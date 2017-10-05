---
title: Azure Active Directory B2B-samarbete koden och PowerShell-exempel | Microsoft Docs
description: "Koden och PowerShell-exempel för Azure Active Directory B2B-samarbete"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: cae69f57627b3058bf96c3d1eea7dadc81147153
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2b-collaboration-code-and-powershell-samples"></a><span data-ttu-id="112b2-103">Azure Active Directory B2B-samarbete koden och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="112b2-103">Azure Active Directory B2B collaboration code and PowerShell samples</span></span>

## <a name="powershell-example"></a><span data-ttu-id="112b2-104">PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="112b2-104">PowerShell example</span></span>
<span data-ttu-id="112b2-105">Du kan bulk-inbjudan externa användare till en organisation från e-postadresser som du har lagrat i en. CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="112b2-105">You can bulk-invite external users to an organization from email addresses that you have stored in a .CSV file.</span></span>

1. <span data-ttu-id="112b2-106">Förbereda den. CSV-filen skapa en ny CSV-fil och ger den namnet invitations.csv.</span><span class="sxs-lookup"><span data-stu-id="112b2-106">Prepare the .CSV file Create a new CSV file and name it invitations.csv.</span></span> <span data-ttu-id="112b2-107">I det här exemplet filen sparas i C:\data och innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="112b2-107">In this example, the file is saved in C:\data, and contains the following information:</span></span>
  
  <span data-ttu-id="112b2-108">Namn</span><span class="sxs-lookup"><span data-stu-id="112b2-108">Name</span></span>                  |  <span data-ttu-id="112b2-109">InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="112b2-109">InvitedUserEmailAddress</span></span>
  --------------------- | --------------------------
  <span data-ttu-id="112b2-110">Gmail B2B bjudits in</span><span class="sxs-lookup"><span data-stu-id="112b2-110">Gmail B2B Invitee</span></span>     | b2binvitee@gmail.com
  <span data-ttu-id="112b2-111">Outlook B2B bjudits in</span><span class="sxs-lookup"><span data-stu-id="112b2-111">Outlook B2B invitee</span></span>   | b2binvitee@outlook.com


2. <span data-ttu-id="112b2-112">Hämta den senaste Azure AD PowerShell för att använda de nya cmdletarna måste du installera den uppdaterade Azure AD PowerShell-modulen som du kan hämta från [Powershell-modulen versionen sida](https://www.powershellgallery.com/packages/AzureADPreview)</span><span class="sxs-lookup"><span data-stu-id="112b2-112">Get the latest Azure AD PowerShell To use the new cmdlets, you must install the updated Azure AD PowerShell module, which you can download from [the Powershell module's release page](https://www.powershellgallery.com/packages/AzureADPreview)</span></span>

3. <span data-ttu-id="112b2-113">Logga in på din innehavare</span><span class="sxs-lookup"><span data-stu-id="112b2-113">Sign in to your tenancy</span></span>

    ```
    $cred = Get-Credential
    Connect-AzureAD -Credential $cred
    ```

4. <span data-ttu-id="112b2-114">Kör PowerShell-cmdlet</span><span class="sxs-lookup"><span data-stu-id="112b2-114">Run the PowerShell cmdlet</span></span>

  ```
  $invitations = import-csv C:\data\invitations.csv
  $messageInfo = New-Object Microsoft.Open.MSGraph.Model.InvitedUserMessageInfo
  $messageInfo.customizedMessageBody = “Hey there! Check this out. I created an invitation through PowerShell”
  foreach ($email in $invitations) {New-AzureADMSInvitation -InvitedUserEmailAddress $email.InvitedUserEmailAddress -InvitedUserDisplayName $email.Name -InviteRedirectUrl https://wingtiptoysonline-dev-ed.my.salesforce.com -InvitedUserMessageInfo $messageInfo -SendInvitationMessage $true}
  ```

<span data-ttu-id="112b2-115">Denna cmdlet skickas en inbjudan till e-postadresser i invitations.csv.</span><span class="sxs-lookup"><span data-stu-id="112b2-115">This cmdlet sends an invitation to the email addresses in invitations.csv.</span></span> <span data-ttu-id="112b2-116">Ytterligare funktioner för denna cmdlet är:</span><span class="sxs-lookup"><span data-stu-id="112b2-116">Additional features of this cmdlet include:</span></span>
- <span data-ttu-id="112b2-117">Anpassad text i e-postmeddelandet</span><span class="sxs-lookup"><span data-stu-id="112b2-117">Customized text in the email message</span></span>
- <span data-ttu-id="112b2-118">Inklusive ett visningsnamn för inbjudna användare</span><span class="sxs-lookup"><span data-stu-id="112b2-118">Including a display name for the invited user</span></span>
- <span data-ttu-id="112b2-119">Skicka meddelanden till CCs eller utelämna e-postmeddelanden helt</span><span class="sxs-lookup"><span data-stu-id="112b2-119">Sending messages to CCs or suppressing email messages altogether</span></span>

## <a name="code-sample"></a><span data-ttu-id="112b2-120">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="112b2-120">Code sample</span></span>
<span data-ttu-id="112b2-121">Här visar vi hur du anropar inbjudan-API: T i ”app” läge, att hämta URL: en åtgärd för den resurs som du bjuder in B2B-användare.</span><span class="sxs-lookup"><span data-stu-id="112b2-121">Here we illustrate how to call the invitation API, in "app-only" mode, to get the redemption URL for the resource to which you are inviting the B2B user.</span></span> <span data-ttu-id="112b2-122">Målet är att skicka ett e-postmeddelande med anpassade inbjudan.</span><span class="sxs-lookup"><span data-stu-id="112b2-122">The goal is to send a custom invitation email.</span></span> <span data-ttu-id="112b2-123">E-postmeddelandet kan sammanställas med HTTP-klienter, så att du kan anpassa hur den ser ut och skicka den via Graph API.</span><span class="sxs-lookup"><span data-stu-id="112b2-123">The email can be composed with an HTTP client, so you can customize how it looks and send it through Graph API.</span></span>

```
namespace SampleInviteApp
{
    using System;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    class Program
    {
        /// <summary>
        /// Microsoft graph resource.
        /// </summary>
        static readonly string GraphResource = "https://graph.microsoft.com";
 
        /// <summary>
        /// Microsoft graph invite endpoint.
        /// </summary>
        static readonly string InviteEndPoint = "https://graph.microsoft.com/v1.0/invitations";
 
        /// <summary>
        ///  Authentication endpoint to get token.
        /// </summary>
        static readonly string EstsLoginEndpoint = "https://login.microsoftonline.com";
 
        /// <summary>
        /// This is the tenantid of the tenant you want to invite users to.
        /// </summary>
        private static readonly string TenantID = "";
 
        /// <summary>
        /// This is the application id of the application that is registered in the above tenant.
        /// The required scopes are available in the below link.
        /// https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/invitation_post
        /// </summary>
        private static readonly string TestAppClientId = "";
 
        /// <summary>
        /// Client secret of the application.
        /// </summary>
        private static readonly string TestAppClientSecret = @"
 
        /// <summary>
        /// This is the email address of the user you want to invite.
        /// </summary>
        private static readonly string InvitedUserEmailAddress = @"";
 
        /// <summary>
        /// This is the display name of the user you want to invite.
        /// </summary>
        private static readonly string InvitedUserDisplayName = @"";
 
        /// <summary>
        /// Main method.
        /// </summary>
        /// <param name="args">Optional arguments</param>
        static void Main(string[] args)
        {
            Invitation invitation = CreateInvitation();
            SendInvitation(invitation);
        }
 
        /// <summary>
        /// Create the invitation object.
        /// </summary>
        /// <returns>Returns the invitation object.</returns>
        private static Invitation CreateInvitation()
        {
            // Set the invitation object.
            Invitation invitation = new Invitation();
            invitation.InvitedUserDisplayName = InvitedUserDisplayName;
            invitation.InvitedUserEmailAddress = InvitedUserEmailAddress;
            invitation.InviteRedirectUrl = "https://www.microsoft.com";
            invitation.SendInvitationMessage = true;
            return invitation;
        }
 
        /// <summary>
        /// Send the guest user invite request.
        /// </summary>
        /// <param name="invitation">Invitation object.</param>
        private static void SendInvitation(Invitation invitation)
        {
            string accessToken = GetAccessToken();
 
            HttpClient httpClient = GetHttpClient(accessToken);
 
            // Make the invite call. 
            HttpContent content = new StringContent(JsonConvert.SerializeObject(invitation));
            content.Headers.Add("ContentType", "application/json");
            var postResponse = httpClient.PostAsync(InviteEndPoint, content).Result;
            string serverResponse = postResponse.Content.ReadAsStringAsync().Result;
            Console.WriteLine(serverResponse);
        }
 
        /// <summary>
        /// Get the HTTP client.
        /// </summary>
        /// <param name="accessToken">Access token</param>
        /// <returns>Returns the Http Client.</returns>
        private static HttpClient GetHttpClient(string accessToken)
        {
            // setup http client.
            HttpClient httpClient = new HttpClient();
            httpClient.Timeout = TimeSpan.FromSeconds(300);
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
            httpClient.DefaultRequestHeaders.Add("client-request-id", Guid.NewGuid().ToString());
            Console.WriteLine(
                "CorrelationID for the request: {0}",
                httpClient.DefaultRequestHeaders.GetValues("client-request-id").Single());
            return httpClient;
        }
 
        /// <summary>
        /// Get the access token for our application to talk to microsoft graph.
        /// </summary>
        /// <returns>Returns the access token for our application to talk to microsoft graph.</returns>
        private static string GetAccessToken()
        {
            string accessToken = null;
 
            // Get the access token for our application to talk to microsoft graph.
            try
            {
                AuthenticationContext testAuthContext =
                    new AuthenticationContext(string.Format("{0}/{1}", EstsLoginEndpoint, TenantID));
                AuthenticationResult testAuthResult = testAuthContext.AcquireTokenAsync(
                    GraphResource,
                    new ClientCredential(TestAppClientId, TestAppClientSecret)).Result;
                accessToken = testAuthResult.AccessToken;
            }
            catch (AdalException ex)
            {
                Console.WriteLine("An exception was thrown while fetching the token: {0}.", ex);
                throw;
            }
 
            return accessToken;
        }
 
        /// <summary>
        /// Invitation class.
        /// </summary>
        public class Invitation
        {
            /// <summary>
            /// Gets or sets display name.
            /// </summary>
            public string InvitedUserDisplayName { get; set; }
 
            /// <summary>
            /// Gets or sets display name.
            /// </summary>
            public string InvitedUserEmailAddress { get; set; }
 
            /// <summary>
            /// Gets or sets a value indicating whether Invitation Manager should send the email to InvitedUser.
            /// </summary>
            public bool SendInvitationMessage { get; set; }
 
            /// <summary>
            /// Gets or sets invitation redirect URL
            /// </summary>
            public string InviteRedirectUrl { get; set; }
        }
    }
}
```


## <a name="next-steps"></a><span data-ttu-id="112b2-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="112b2-124">Next steps</span></span>

<span data-ttu-id="112b2-125">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="112b2-125">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="112b2-126">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="112b2-126">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="112b2-127">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="112b2-127">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="112b2-128">Lägger till en B2B-samarbete användare till en roll</span><span class="sxs-lookup"><span data-stu-id="112b2-128">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="112b2-129">Delegera B2B-samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="112b2-129">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="112b2-130">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="112b2-130">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="112b2-131">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="112b2-131">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="112b2-132">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="112b2-132">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="112b2-133">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="112b2-133">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="112b2-134">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="112b2-134">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="112b2-135">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="112b2-135">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
