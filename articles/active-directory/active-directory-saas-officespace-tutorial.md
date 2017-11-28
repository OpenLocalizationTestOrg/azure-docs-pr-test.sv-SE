---
title: "Självstudier: Azure Active Directory-integrering med OfficeSpace programvara | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan OfficeSpace programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 43d2ecfe851d8f6c43cd4ce7fc4bd872818f4137
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a><span data-ttu-id="f3a9a-103">Självstudier: Azure Active Directory-integrering med OfficeSpace programvara</span><span class="sxs-lookup"><span data-stu-id="f3a9a-103">Tutorial: Azure Active Directory integration with OfficeSpace Software</span></span>

<span data-ttu-id="f3a9a-104">I kursen får lära du att integrera OfficeSpace programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f3a9a-104">In this tutorial, you learn how to integrate OfficeSpace Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f3a9a-105">Integrera OfficeSpace programvara med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-105">Integrating OfficeSpace Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f3a9a-106">Du kan styra i Azure AD som har åtkomst till OfficeSpace programvara.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-106">You can control in Azure AD who has access to OfficeSpace Software.</span></span>
- <span data-ttu-id="f3a9a-107">Du kan aktivera användarna att automatiskt hämta loggat in på OfficeSpace programvara (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-107">You can enable your users to automatically get signed-on to OfficeSpace Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f3a9a-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="f3a9a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f3a9a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3a9a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f3a9a-110">Prerequisites</span></span>

<span data-ttu-id="f3a9a-111">För att konfigurera Azure AD-integrering med OfficeSpace programvara, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-111">To configure Azure AD integration with OfficeSpace Software, you need the following items:</span></span>

- <span data-ttu-id="f3a9a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f3a9a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f3a9a-113">En OfficeSpace programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f3a9a-113">A OfficeSpace Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f3a9a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f3a9a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f3a9a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f3a9a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f3a9a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f3a9a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f3a9a-118">Scenario description</span></span>
<span data-ttu-id="f3a9a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f3a9a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f3a9a-121">Lägga till OfficeSpace programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="f3a9a-121">Adding OfficeSpace Software from the gallery</span></span>
2. <span data-ttu-id="f3a9a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3a9a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-officespace-software-from-the-gallery"></a><span data-ttu-id="f3a9a-123">Lägga till OfficeSpace programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="f3a9a-123">Adding OfficeSpace Software from the gallery</span></span>
<span data-ttu-id="f3a9a-124">Du måste lägga till OfficeSpace programvara från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av OfficeSpace programvara i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-124">To configure the integration of OfficeSpace Software into Azure AD, you need to add OfficeSpace Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f3a9a-125">**Utför följande steg för att lägga till OfficeSpace programvara från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f3a9a-125">**To add OfficeSpace Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f3a9a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="f3a9a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f3a9a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="f3a9a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="f3a9a-133">I sökrutan skriver **OfficeSpace programvara**väljer **OfficeSpace programvara** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-133">In the search box, type **OfficeSpace Software**, select **OfficeSpace Software** from result panel then click **Add** button to add the application.</span></span>

    ![OfficeSpace programvara i resultatlistan](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f3a9a-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3a9a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f3a9a-136">Du konfigurera och testa Azure AD enkel inloggning med OfficeSpace programvara baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-136">In this section, you configure and test Azure AD single sign-on with OfficeSpace Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f3a9a-137">Azure AD måste du känna till motsvarande användaren i OfficeSpace programvara till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in OfficeSpace Software is to a user in Azure AD.</span></span> <span data-ttu-id="f3a9a-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i OfficeSpace programvara upprättas.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-138">In other words, a link relationship between an Azure AD user and the related user in OfficeSpace Software needs to be established.</span></span>

<span data-ttu-id="f3a9a-139">I OfficeSpace programvara, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-139">In OfficeSpace Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f3a9a-140">Om du vill konfigurera och testa Azure AD enkel inloggning med OfficeSpace programvara, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-140">To configure and test Azure AD single sign-on with OfficeSpace Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f3a9a-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f3a9a-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f3a9a-143">**[Skapa en testanvändare OfficeSpace programvara](#create-a-officespace-software-test-user)**  – du har en motsvarighet för Britta Simon OfficeSpace programvara som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-143">**[Create a OfficeSpace Software test user](#create-a-officespace-software-test-user)** - to have a counterpart of Britta Simon in OfficeSpace Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f3a9a-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f3a9a-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f3a9a-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3a9a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f3a9a-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i OfficeSpace programmet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your OfficeSpace Software application.</span></span>

<span data-ttu-id="f3a9a-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med OfficeSpace programvara:**</span><span class="sxs-lookup"><span data-stu-id="f3a9a-148">**To configure Azure AD single sign-on with OfficeSpace Software, perform the following steps:**</span></span>

1. <span data-ttu-id="f3a9a-149">I Azure-portalen på den **OfficeSpace programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-149">In the Azure portal, on the **OfficeSpace Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="f3a9a-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. <span data-ttu-id="f3a9a-153">På den **OfficeSpace programvara domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-153">On the **OfficeSpace Software Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och OfficeSpace programvara domän med enkel inloggning information](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    <span data-ttu-id="f3a9a-155">a.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-155">a.</span></span> <span data-ttu-id="f3a9a-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.officespacesoftware.com/users/sign_in/saml`</span><span class="sxs-lookup"><span data-stu-id="f3a9a-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.officespacesoftware.com/users/sign_in/saml`</span></span>

    <span data-ttu-id="f3a9a-157">b.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-157">b.</span></span> <span data-ttu-id="f3a9a-158">I den **identifierare** textruta Skriv en URL med följande mönster:`<company name>.officespacesoftware.com`</span><span class="sxs-lookup"><span data-stu-id="f3a9a-158">In the **Identifier** textbox, type a URL using the following pattern: `<company name>.officespacesoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f3a9a-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-159">These values are not real.</span></span> <span data-ttu-id="f3a9a-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f3a9a-161">Kontakta [OfficeSpace Programvaruklienten supportteamet](mailto:support@officespacesoftware.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-161">Contact [OfficeSpace Software Client support team](mailto:support@officespacesoftware.com) to get these values.</span></span> 

4. <span data-ttu-id="f3a9a-162">OfficeSpace program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-162">OfficeSpace Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="f3a9a-163">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-163">Please configure the following claims for this application.</span></span> <span data-ttu-id="f3a9a-164">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-164">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="f3a9a-165">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-165">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurera attribut](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. <span data-ttu-id="f3a9a-167">I den **användarattribut** avsnitt på den **enkel inloggning** markerar **user.mail** som **användar-ID** och för varje rad som visas i i tabellen nedan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-167">In the **User Attributes** section on the **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in the table below, perform the following steps:</span></span>
    
    | <span data-ttu-id="f3a9a-168">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="f3a9a-168">Attribute Name</span></span> | <span data-ttu-id="f3a9a-169">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="f3a9a-169">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="f3a9a-170">E-post</span><span class="sxs-lookup"><span data-stu-id="f3a9a-170">email</span></span> | <span data-ttu-id="f3a9a-171">User.Mail</span><span class="sxs-lookup"><span data-stu-id="f3a9a-171">user.mail</span></span> |
    | <span data-ttu-id="f3a9a-172">namn</span><span class="sxs-lookup"><span data-stu-id="f3a9a-172">name</span></span> | <span data-ttu-id="f3a9a-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="f3a9a-173">user.displayname</span></span> |
    | <span data-ttu-id="f3a9a-174">Förnamn</span><span class="sxs-lookup"><span data-stu-id="f3a9a-174">first_name</span></span> | <span data-ttu-id="f3a9a-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="f3a9a-175">user.givenname</span></span> |
    | <span data-ttu-id="f3a9a-176">Efternamn</span><span class="sxs-lookup"><span data-stu-id="f3a9a-176">last_name</span></span> | <span data-ttu-id="f3a9a-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="f3a9a-177">user.surname</span></span> |

    <span data-ttu-id="f3a9a-178">a.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-178">a.</span></span> <span data-ttu-id="f3a9a-179">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![<span data-ttu-id="f3a9a-180">Konfigurera Lägg till</span><span class="sxs-lookup"><span data-stu-id="f3a9a-180">Configure Add</span></span> ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Konfigurera attribut](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="f3a9a-182">b.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-182">b.</span></span> <span data-ttu-id="f3a9a-183">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="f3a9a-184">c.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-184">c.</span></span> <span data-ttu-id="f3a9a-185">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="f3a9a-186">d.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-186">d.</span></span> <span data-ttu-id="f3a9a-187">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="f3a9a-187">Click **Ok**</span></span>
 
6. <span data-ttu-id="f3a9a-188">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-188">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of the certificate.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. <span data-ttu-id="f3a9a-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f3a9a-192">På den **OfficeSpace programvarukonfiguration** klickar du på **konfigurera OfficeSpace programvara** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-192">On the **OfficeSpace Software Configuration** section, click **Configure OfficeSpace Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f3a9a-193">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f3a9a-193">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfiguration av OfficeSpace programvara](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. <span data-ttu-id="f3a9a-195">Logga in på din OfficeSpace programvara klient som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-195">In a different web browser window, log into your OfficeSpace Software tenant as an administrator.</span></span>

10. <span data-ttu-id="f3a9a-196">Gå till **inställningar** och på **kopplingar**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-196">Go to **Settings** and click **Connectors**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. <span data-ttu-id="f3a9a-198">Klicka på **SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-198">Click **SAML Authentication**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. <span data-ttu-id="f3a9a-200">I den **SAML-autentisering** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-200">In the **SAML Authentication** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    <span data-ttu-id="f3a9a-202">a.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-202">a.</span></span> <span data-ttu-id="f3a9a-203">I den **logga ut providern url** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-203">In the **Logout provider url** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f3a9a-204">b.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-204">b.</span></span> <span data-ttu-id="f3a9a-205">I den **klienten idp mål-url** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-205">In the **Client idp target url** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f3a9a-206">c.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-206">c.</span></span> <span data-ttu-id="f3a9a-207">Klistra in den **tumavtrycket** värde som du har kopierat från Azure-portalen i den **klienten IDP certifikat fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-207">Paste the **Thumbprint** value which you have copied from Azure portal, into the **Client IDP certificate fingerprint** textbox.</span></span> 

    <span data-ttu-id="f3a9a-208">d.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-208">d.</span></span> <span data-ttu-id="f3a9a-209">Klicka på **Spara inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-209">Click **Save Settings**.</span></span>


> [!TIP]
> <span data-ttu-id="f3a9a-210">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f3a9a-210">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f3a9a-211">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-211">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f3a9a-212">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f3a9a-212">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f3a9a-213">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3a9a-213">Create an Azure AD test user</span></span>

<span data-ttu-id="f3a9a-214">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-214">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="f3a9a-216">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f3a9a-216">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f3a9a-217">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-217">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f3a9a-219">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-219">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f3a9a-221">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-221">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f3a9a-223">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f3a9a-223">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f3a9a-225">a.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-225">a.</span></span> <span data-ttu-id="f3a9a-226">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-226">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f3a9a-227">b.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-227">b.</span></span> <span data-ttu-id="f3a9a-228">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-228">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="f3a9a-229">c.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-229">c.</span></span> <span data-ttu-id="f3a9a-230">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-230">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="f3a9a-231">d.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-231">d.</span></span> <span data-ttu-id="f3a9a-232">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-232">Click **Create**.</span></span>
 
### <a name="create-a-officespace-software-test-user"></a><span data-ttu-id="f3a9a-233">Skapa en testanvändare OfficeSpace programvara</span><span class="sxs-lookup"><span data-stu-id="f3a9a-233">Create a OfficeSpace Software test user</span></span>

<span data-ttu-id="f3a9a-234">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon OfficeSpace programvaran.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-234">The objective of this section is to create a user called Britta Simon in OfficeSpace Software.</span></span> <span data-ttu-id="f3a9a-235">OfficeSpace programvara stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-235">OfficeSpace Software supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="f3a9a-236">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-236">There is no action item for you in this section.</span></span> <span data-ttu-id="f3a9a-237">En ny användare skapas vid ett försök att komma åt OfficeSpace programvara om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-237">A new user will be created during an attempt to access OfficeSpace Software if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="f3a9a-238">Om du behöver skapa en användare manuellt måste du kontakta [OfficeSpace programvara supportteamet](mailto:support@officespacesoftware.com).</span><span class="sxs-lookup"><span data-stu-id="f3a9a-238">If you need to create an user manually, you need to Contact [OfficeSpace Software support team](mailto:support@officespacesoftware.com).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="f3a9a-239">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f3a9a-239">Assign the Azure AD test user</span></span>

<span data-ttu-id="f3a9a-240">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till OfficeSpace programvara.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to OfficeSpace Software.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="f3a9a-242">**Om du vill tilldela OfficeSpace programvara Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f3a9a-242">**To assign Britta Simon to OfficeSpace Software, perform the following steps:**</span></span>

1. <span data-ttu-id="f3a9a-243">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f3a9a-245">Välj i listan med program **OfficeSpace programvara**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-245">In the applications list, select **OfficeSpace Software**.</span></span>

    ![Länken OfficeSpace programvara i listan med program](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. <span data-ttu-id="f3a9a-247">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="f3a9a-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-249">Click **Add** button.</span></span> <span data-ttu-id="f3a9a-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="f3a9a-252">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f3a9a-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f3a9a-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f3a9a-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3a9a-255">Test single sign-on</span></span>

<span data-ttu-id="f3a9a-256">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f3a9a-257">När du klickar på panelen OfficeSpace programvara på åtkomstpanelen du bör få automatiskt loggat in på OfficeSpace programmet.</span><span class="sxs-lookup"><span data-stu-id="f3a9a-257">When you click the OfficeSpace Software tile in the Access Panel, you should get automatically signed-on to your OfficeSpace Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3a9a-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f3a9a-258">Additional resources</span></span>

* [<span data-ttu-id="f3a9a-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3a9a-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f3a9a-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f3a9a-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

