---
title: "MFA SDK för anpassade appar | Microsoft Docs"
description: "Den här artikeln visar hur du hämtar och använder Azure MFA SDK för att aktivera tvåstegsverifiering för dina egna appar."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 281f9c61a30a20027f69808600373aa272255ef6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a><span data-ttu-id="738f7-103">Skapa Multifaktorautentisering i anpassade appar (SDK)</span><span class="sxs-lookup"><span data-stu-id="738f7-103">Building Multi-Factor Authentication into Custom Apps (SDK)</span></span>

<span data-ttu-id="738f7-104">Azure Multi-Factor Authentication Software Development Kit (SDK) kan du skapa tvåstegsverifiering direkt i inloggning eller transaktionen processer för program i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="738f7-104">The Azure Multi-Factor Authentication Software Development Kit (SDK) lets you build two-step verification directly into the sign-in or transaction processes of applications in your Azure AD tenant.</span></span>

<span data-ttu-id="738f7-105">Multi-Factor Authentication SDK är tillgängligt för C#, Visual Basic (.NET), Java, Perl, PHP och Ruby.</span><span class="sxs-lookup"><span data-stu-id="738f7-105">The Multi-Factor Authentication SDK is available for C#, Visual Basic (.NET), Java, Perl, PHP, and Ruby.</span></span> <span data-ttu-id="738f7-106">SDK innehåller en tunn wrapper runt tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="738f7-106">The SDK provides a thin wrapper around two-step verification.</span></span> <span data-ttu-id="738f7-107">Den innehåller allt du behöver skriva koden, inklusive kommenterad källkodsfiler, exempel filer och en detaljerad viktigt-fil.</span><span class="sxs-lookup"><span data-stu-id="738f7-107">It includes everything you need to write your code, including commented source code files, example files, and a detailed ReadMe file.</span></span> <span data-ttu-id="738f7-108">Varje SDK innehåller också ett certifikat och privat nyckel för att kryptera transaktioner som är unika för multi-Factor Authentication Provider.</span><span class="sxs-lookup"><span data-stu-id="738f7-108">Each SDK also includes a certificate and private key for encrypting transactions that are unique to your Multi-Factor Authentication Provider.</span></span> <span data-ttu-id="738f7-109">Så länge som du har en provider, kan du hämta SDK i så många språk och format som du behöver.</span><span class="sxs-lookup"><span data-stu-id="738f7-109">As long as you have a provider, you can download the SDK in as many languages and formats as you need.</span></span>

<span data-ttu-id="738f7-110">Strukturen för API: er i Multi-Factor Authentication SDK är enkelt.</span><span class="sxs-lookup"><span data-stu-id="738f7-110">The structure of the APIs in the Multi-Factor Authentication SDK is simple.</span></span> <span data-ttu-id="738f7-111">Gör en enskild funktion anrop till en API med Multi-Factor alternativet parametrar (t.ex. verifieringen läge) och användardata (till exempel den telefonnummer eller PIN-kod för att verifiera).</span><span class="sxs-lookup"><span data-stu-id="738f7-111">Make a single function call to an API with the multi-factor option parameters (like verification mode) and user data (like the telephone number to call or the PIN number to validate).</span></span> <span data-ttu-id="738f7-112">API: erna översätta funktionsanropet i tjänster webbegäranden till molnbaserad Azure Multi-Factor Authentication-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="738f7-112">The APIs translate the function call into web services requests to the cloud-based Azure Multi-Factor Authentication Service.</span></span> <span data-ttu-id="738f7-113">Alla anrop måste innehålla en referens till det privata certifikat som ingår i varje SDK.</span><span class="sxs-lookup"><span data-stu-id="738f7-113">All calls must include a reference to the private certificate that is included in every SDK.</span></span>

<span data-ttu-id="738f7-114">Eftersom API: erna inte har åtkomst till användare som har registrerats i Azure Active Directory, måste du ange användarinformation i en fil eller databas.</span><span class="sxs-lookup"><span data-stu-id="738f7-114">Because the APIs do not have access to users registered in Azure Active Directory, you must provide user information in a file or database.</span></span> <span data-ttu-id="738f7-115">Dessutom tillhandahåller API: er funktioner för registrering eller användare, så du måste skapa de här processerna i ditt program.</span><span class="sxs-lookup"><span data-stu-id="738f7-115">Also, the APIs do not provide enrollment or user management features, so you need to build these processes into your application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="738f7-116">För att hämta SDK:n måste du skapa en Azure Multi-Factor Auth-provider, även om du har Azure MFA-, AAD Premium- eller EMS-licenser.</span><span class="sxs-lookup"><span data-stu-id="738f7-116">To download the SDK, you need to create an Azure Multi-Factor Auth Provider even if you have Azure MFA, AAD Premium, or EMS licenses.</span></span> <span data-ttu-id="738f7-117">Om du skapar ett Azure leverantör av Multifaktorautent för detta ändamål och redan har licenser, se till att skapa Provider med den **Per aktiverad användare** modell.</span><span class="sxs-lookup"><span data-stu-id="738f7-117">If you create an Azure Multi-Factor Auth Provider for this purpose and already have licenses, make sure to create the Provider with the **Per Enabled User** model.</span></span> <span data-ttu-id="738f7-118">Koppla sedan providern till katalogen som innehåller Azure MFA, Azure AD Premium eller EMS-licenser.</span><span class="sxs-lookup"><span data-stu-id="738f7-118">Then, link the Provider to the directory that contains the Azure MFA, Azure AD Premium, or EMS licenses.</span></span> <span data-ttu-id="738f7-119">Den här konfigurationen garanterar att du endast debiteras om du har flera unika användare med hjälp av SDK än antalet licenser du äger.</span><span class="sxs-lookup"><span data-stu-id="738f7-119">This configuration ensures that you are only billed if you have more unique users using the SDK than the number of licenses you own.</span></span>


## <a name="download-the-sdk"></a><span data-ttu-id="738f7-120">Ladda ned SDK</span><span class="sxs-lookup"><span data-stu-id="738f7-120">Download the SDK</span></span>
<span data-ttu-id="738f7-121">Hämta SDK för Azure Multi-Factor kräver en [Azure leverantör av Multifaktorautent](multi-factor-authentication-get-started-auth-provider.md).</span><span class="sxs-lookup"><span data-stu-id="738f7-121">Downloading the Azure Multi-Factor SDK requires an [Azure Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md).</span></span>  <span data-ttu-id="738f7-122">Detta kräver en fullständig Azure-prenumeration, även om Azure MFA, Azure AD Premium eller Enterprise Mobility Suite licenser äger.</span><span class="sxs-lookup"><span data-stu-id="738f7-122">This requires a full Azure subscription, even if Azure MFA, Azure AD Premium, or Enterprise Mobility Suite licenses are owned.</span></span>  <span data-ttu-id="738f7-123">Navigera till Multi-Factor-hanteringsportalen om du vill ladda ned SDK.</span><span class="sxs-lookup"><span data-stu-id="738f7-123">To download the SDK, navigate to the Multi-Factor Management Portal.</span></span> <span data-ttu-id="738f7-124">Du kan komma åt portalen genom att hantera den leverantör av Multifaktorautent direkt eller genom att klicka på den **”gå till portalen”** länk på inställningssidan för MFA-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="738f7-124">You can reach the portal either by managing the Multi-Factor Auth Provider directly, or by clicking the **"Go to the portal"** link on the MFA service settings page.</span></span>

### <a name="download-from-the-azure-classic-portal"></a><span data-ttu-id="738f7-125">Ladda ned från den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="738f7-125">Download from the Azure classic portal</span></span>
1. <span data-ttu-id="738f7-126">Logga in på [den klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="738f7-126">Sign in to the [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="738f7-127">Välj **Active Directory** till vänster.</span><span class="sxs-lookup"><span data-stu-id="738f7-127">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="738f7-128">På sidan Active Directory på den översta väljer **Flerfunktionsautentiseringsleverantörer**</span><span class="sxs-lookup"><span data-stu-id="738f7-128">On the Active Directory page, at the top select **Multi-Factor Auth Providers**</span></span>
4. <span data-ttu-id="738f7-129">Markera längst **hantera**.</span><span class="sxs-lookup"><span data-stu-id="738f7-129">At the bottom select **Manage**.</span></span> <span data-ttu-id="738f7-130">En ny sida öppnas.</span><span class="sxs-lookup"><span data-stu-id="738f7-130">A new page opens.</span></span>
5. <span data-ttu-id="738f7-131">Klicka på vänster längst ned, **SDK**.</span><span class="sxs-lookup"><span data-stu-id="738f7-131">On the left, at the bottom, click **SDK**.</span></span>
   <span data-ttu-id="738f7-132"><center>![Ladda ned](./media/multi-factor-authentication-sdk/download.png)</center></span><span class="sxs-lookup"><span data-stu-id="738f7-132"><center>![Download](./media/multi-factor-authentication-sdk/download.png)</center></span></span>
6. <span data-ttu-id="738f7-133">Välj önskat språk och klicka på en de associera länkarna.</span><span class="sxs-lookup"><span data-stu-id="738f7-133">Select the language you want and click one the associated download links.</span></span>
7. <span data-ttu-id="738f7-134">Spara den nedladdade filen.</span><span class="sxs-lookup"><span data-stu-id="738f7-134">Save the download.</span></span>

### <a name="download-from-the-service-settings"></a><span data-ttu-id="738f7-135">Ladda ned från tjänstinställningar</span><span class="sxs-lookup"><span data-stu-id="738f7-135">Download from the service settings</span></span>
1. <span data-ttu-id="738f7-136">Logga in på [den klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="738f7-136">Sign in to the [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="738f7-137">Välj **Active Directory** till vänster.</span><span class="sxs-lookup"><span data-stu-id="738f7-137">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="738f7-138">Dubbelklicka på din instans av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="738f7-138">Double-click your instance of Azure AD.</span></span>
4. <span data-ttu-id="738f7-139">Klicka på **Konfigurera** längst upp.</span><span class="sxs-lookup"><span data-stu-id="738f7-139">At the top click **Configure**</span></span>
5. <span data-ttu-id="738f7-140">Välj under multifaktorautentisering, **hantera tjänstinställningar**
   ![hämta](./media/multi-factor-authentication-sdk/download2.png)</span><span class="sxs-lookup"><span data-stu-id="738f7-140">Under multi-factor authentication, select **Manage service settings**
![Download](./media/multi-factor-authentication-sdk/download2.png)</span></span>
6. <span data-ttu-id="738f7-141">På sidan för tjänstinställningar klickar du på **Gå till portalen** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="738f7-141">On the services settings page, at the bottom of the screen click **Go to the portal**.</span></span> <span data-ttu-id="738f7-142">En ny sida öppnas.</span><span class="sxs-lookup"><span data-stu-id="738f7-142">A new page opens.</span></span>
   <span data-ttu-id="738f7-143">![Ladda ned](./media/multi-factor-authentication-sdk/download3a.png)</span><span class="sxs-lookup"><span data-stu-id="738f7-143">![Download](./media/multi-factor-authentication-sdk/download3a.png)</span></span>
7. <span data-ttu-id="738f7-144">Klicka på vänster längst ned, **SDK**.</span><span class="sxs-lookup"><span data-stu-id="738f7-144">On the left, at the bottom, click **SDK**.</span></span>
8. <span data-ttu-id="738f7-145">Välj önskat språk och klicka på en de associera länkarna.</span><span class="sxs-lookup"><span data-stu-id="738f7-145">Select the language you want and click one the associated download links.</span></span>
9. <span data-ttu-id="738f7-146">Spara den nedladdade filen.</span><span class="sxs-lookup"><span data-stu-id="738f7-146">Save the download.</span></span>

## <a name="whats-in-the-sdk"></a><span data-ttu-id="738f7-147">Vad är i SDK</span><span class="sxs-lookup"><span data-stu-id="738f7-147">What's in the SDK</span></span>
<span data-ttu-id="738f7-148">SDK innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="738f7-148">The SDK includes the following items:</span></span>

* <span data-ttu-id="738f7-149">**VIKTIGT**.</span><span class="sxs-lookup"><span data-stu-id="738f7-149">**README**.</span></span> <span data-ttu-id="738f7-150">Förklarar hur du använder multi-Factor Authentication-API: er i ett nytt eller befintligt program.</span><span class="sxs-lookup"><span data-stu-id="738f7-150">Explains how to use the Multi-Factor Authentication APIs in a new or existing application.</span></span>
* <span data-ttu-id="738f7-151">**Källfiler** för Multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="738f7-151">**Source files** for Multi-Factor Authentication</span></span>
* <span data-ttu-id="738f7-152">**Klientcertifikatet** som används för att kommunicera med Multi-Factor Authentication-tjänsten</span><span class="sxs-lookup"><span data-stu-id="738f7-152">**Client certificate** that you use to communicate with the Multi-Factor Authentication service</span></span>
* <span data-ttu-id="738f7-153">**Privat nyckel** för certifikatet</span><span class="sxs-lookup"><span data-stu-id="738f7-153">**Private key** for the certificate</span></span>
* <span data-ttu-id="738f7-154">**Anropa resultat.**</span><span class="sxs-lookup"><span data-stu-id="738f7-154">**Call results.**</span></span> <span data-ttu-id="738f7-155">En lista över resultatkoder för webbtjänstanrop.</span><span class="sxs-lookup"><span data-stu-id="738f7-155">A list of call result codes.</span></span> <span data-ttu-id="738f7-156">För att öppna den här filen genom att använda ett program med textformatering, till exempel WordPad.</span><span class="sxs-lookup"><span data-stu-id="738f7-156">To open this file, use an application with text formatting, such as WordPad.</span></span> <span data-ttu-id="738f7-157">Använd resultatkoder för anrop för att testa och felsöka implementeringen av Multi-Factor Authentication i ditt program.</span><span class="sxs-lookup"><span data-stu-id="738f7-157">Use the call result codes to test and troubleshoot the implementation of Multi-Factor Authentication in your application.</span></span> <span data-ttu-id="738f7-158">De är inte statuskoder för autentisering.</span><span class="sxs-lookup"><span data-stu-id="738f7-158">They are not authentication status codes.</span></span>
* <span data-ttu-id="738f7-159">**Exempel.**</span><span class="sxs-lookup"><span data-stu-id="738f7-159">**Examples.**</span></span> <span data-ttu-id="738f7-160">Exempelkod för en grundläggande fungerande implementeringen av Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="738f7-160">Sample code for a basic working implementation of Multi-Factor Authentication.</span></span>

> [!WARNING]
> <span data-ttu-id="738f7-161">Klientcertifikatet är en unik privat certifikat som har genererats särskilt för dig.</span><span class="sxs-lookup"><span data-stu-id="738f7-161">The client certificate is a unique private certificate that was generated especially for you.</span></span> <span data-ttu-id="738f7-162">Dela eller inte förlorar den här filen.</span><span class="sxs-lookup"><span data-stu-id="738f7-162">Do not share or lose this file.</span></span> <span data-ttu-id="738f7-163">Det är din nyckel för att säkerställa säkerheten för din kommunikation med Multi-Factor Authentication-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="738f7-163">It’s your key to ensuring the security of your communications with the Multi-Factor Authentication service.</span></span>

## <a name="code-sample"></a><span data-ttu-id="738f7-164">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="738f7-164">Code sample</span></span>
<span data-ttu-id="738f7-165">Det här kodexemplet visar hur du använder API: erna i Azure Multi-Factor Authentication SDK för att lägga till standardläge röst anropet verifiering i ditt program.</span><span class="sxs-lookup"><span data-stu-id="738f7-165">This code sample shows you how to use the APIs in the Azure Multi-Factor Authentication SDK to add standard mode voice call verification to your application.</span></span> <span data-ttu-id="738f7-166">Standardläge är ett telefonsamtal som användaren svarar genom att trycka på #.</span><span class="sxs-lookup"><span data-stu-id="738f7-166">Standard mode is a telephone call that the user responds to by pressing the # key.</span></span>

<span data-ttu-id="738f7-167">Det här exemplet använder C# .NET 2.0 Multi-Factor Authentication SDK i ett grundläggande ASP.NET-program med C# serversidan logik, men processen påminner om på andra språk.</span><span class="sxs-lookup"><span data-stu-id="738f7-167">This example uses the C# .NET 2.0 Multi-Factor Authentication SDK in a basic ASP.NET application with C# server-side logic, but the process is similar in other languages.</span></span> <span data-ttu-id="738f7-168">Eftersom SDK innehåller källfiler, inte körbara filer kan skapa filer och referera till dem eller lägga till dem direkt i ditt program.</span><span class="sxs-lookup"><span data-stu-id="738f7-168">Because the SDK includes source files, not executable files, you can build the files and reference them or include them directly in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="738f7-169">När du implementerar Multifaktorautentisering använda metoderna ytterligare (telefonsamtal eller SMS) som sekundär eller tertiär verifiering för att komplettera primära autentiseringsmetod (användarnamn och lösenord).</span><span class="sxs-lookup"><span data-stu-id="738f7-169">When implementing Multi-Factor Authentication, use the additional methods (phone call or text message) as secondary or tertiary verification to supplement your primary authentication method (username and password).</span></span> <span data-ttu-id="738f7-170">Dessa metoder är inte avsedda som primära autentiseringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="738f7-170">These methods are not designed as primary authentication methods.</span></span>

### <a name="code-sample-overview"></a><span data-ttu-id="738f7-171">Översikt över kod-exempel</span><span class="sxs-lookup"><span data-stu-id="738f7-171">Code Sample Overview</span></span>
<span data-ttu-id="738f7-172">Den här exempelkoden för en enkel demo-webbapp använder ett telefonsamtal med svaret # för att verifiera användarautentiseringen.</span><span class="sxs-lookup"><span data-stu-id="738f7-172">This sample code for a simple web demo application uses a telephone call with a # key response to verify the user's authentication.</span></span> <span data-ttu-id="738f7-173">Denna telefonsamtal faktor kallas i Multi-Factor Authentication standardläge.</span><span class="sxs-lookup"><span data-stu-id="738f7-173">This telephone call factor is known in Multi-Factor Authentication as standard mode.</span></span>

<span data-ttu-id="738f7-174">Koden för klientsidan innehåller inte några Multi-Factor Authentication-specifika element.</span><span class="sxs-lookup"><span data-stu-id="738f7-174">The client-side code does not include any Multi-Factor Authentication-specific elements.</span></span> <span data-ttu-id="738f7-175">Eftersom ytterligare autentiseringsfaktorer är oberoende av den primära autentiseringen, du kan lägga till dem utan att ändra befintlig inloggning gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="738f7-175">Because the additional authentication factors are independent of the primary authentication, you can add them without changing the existing sign-on interface.</span></span> <span data-ttu-id="738f7-176">API: er i Multi-Factor SDK kan du anpassa användarupplevelsen, men du kanske inte behöver ändra något alls.</span><span class="sxs-lookup"><span data-stu-id="738f7-176">The APIs in the Multi-Factor SDK let you customize the user experience, but you might not need to change anything at all.</span></span>

<span data-ttu-id="738f7-177">Serverkoden som lägger till standard-autentisering i steg 2.</span><span class="sxs-lookup"><span data-stu-id="738f7-177">The server-side code adds standard-mode authentication in Step 2.</span></span> <span data-ttu-id="738f7-178">Den skapar ett PfAuthParams-objekt med de parametrar som krävs för verifiering i standard-läge: användarnamn, telefon siffra, och läge och sökvägen till klientcertifikatet (CertFilePath) som krävs i varje anrop.</span><span class="sxs-lookup"><span data-stu-id="738f7-178">It creates a PfAuthParams object with the parameters that are required for standard-mode verification: username, telephone number, and mode, and the path to the client certificate (CertFilePath), which is required in each call.</span></span> <span data-ttu-id="738f7-179">En demonstration av alla parametrar i PfAuthParams finns i SDK-exempelfil.</span><span class="sxs-lookup"><span data-stu-id="738f7-179">For a demonstration of all parameters in PfAuthParams, see the Example file in the SDK.</span></span>

<span data-ttu-id="738f7-180">Koden skickar sedan PfAuthParams objektet till funktionen pf_authenticate().</span><span class="sxs-lookup"><span data-stu-id="738f7-180">Next, the code passes the PfAuthParams object to the pf_authenticate() function.</span></span> <span data-ttu-id="738f7-181">Det returnera värdet anger lyckad eller misslyckad verifiering.</span><span class="sxs-lookup"><span data-stu-id="738f7-181">The return value indicates the success or failure of the authentication.</span></span> <span data-ttu-id="738f7-182">Den out-parametrar, callStatus och samtalsstatus kan innehålla ytterligare information om resultatet.</span><span class="sxs-lookup"><span data-stu-id="738f7-182">The out parameters, callStatus, and errorID, contain additional call result information.</span></span> <span data-ttu-id="738f7-183">Resultatkoder för webbtjänstanrop dokumenteras i resultatfilen anrop i SDK.</span><span class="sxs-lookup"><span data-stu-id="738f7-183">The call result codes are documented in the call results file in the SDK.</span></span>

<span data-ttu-id="738f7-184">Den här minimal implementeringen kan skrivas i några rader.</span><span class="sxs-lookup"><span data-stu-id="738f7-184">This minimal implementation can be written in a few lines.</span></span> <span data-ttu-id="738f7-185">I produktionskod, skulle du dock inkludera mer felhantering, ytterligare databaskod och en förbättrad användarupplevelse.</span><span class="sxs-lookup"><span data-stu-id="738f7-185">However, in production code, you would include more sophisticated error handling, additional database code, and an enhanced user experience.</span></span>

### <a name="web-client-code"></a><span data-ttu-id="738f7-186">Web klientkod</span><span class="sxs-lookup"><span data-stu-id="738f7-186">Web Client Code</span></span>
<span data-ttu-id="738f7-187">Följande är web klientkod för en Demonstrationssida.</span><span class="sxs-lookup"><span data-stu-id="738f7-187">The following is web client code for a demo page.</span></span>

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a><span data-ttu-id="738f7-188">Kod på serversidan</span><span class="sxs-lookup"><span data-stu-id="738f7-188">Server-Side Code</span></span>
<span data-ttu-id="738f7-189">I följande kod för servern, har Multi-Factor Authentication konfigurerats och kör i steg 2.</span><span class="sxs-lookup"><span data-stu-id="738f7-189">In the following server-side code, Multi-Factor Authentication is configured and run in Step 2.</span></span> <span data-ttu-id="738f7-190">Standardläge (MODE_STANDARD) är ett telefonsamtal som användaren svarar genom att trycka på #.</span><span class="sxs-lookup"><span data-stu-id="738f7-190">Standard mode (MODE_STANDARD) is a telephone call to which the user responds by pressing the # key.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
