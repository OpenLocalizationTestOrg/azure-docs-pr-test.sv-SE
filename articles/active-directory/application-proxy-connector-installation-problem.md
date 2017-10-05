---
title: Problem med att installera agenten Application Proxy Connector | Microsoft Docs
description: "Felsökning av problem som kan uppstår när du installerar agenten Application Proxy Connector"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 91b3f6f3c8339647f568a509e9efd8e1fffb13dd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a><span data-ttu-id="73014-103">Problem med att installera agenten Application Proxy Connector</span><span class="sxs-lookup"><span data-stu-id="73014-103">Problem installing the Application Proxy Agent Connector</span></span>

<span data-ttu-id="73014-104">Microsoft AAD Application Proxy Connector är en intern domän-komponent som använder utgående anslutningar för att upprätta en anslutning från molnet tillgängliga slutpunkten till den interna domänen.</span><span class="sxs-lookup"><span data-stu-id="73014-104">Microsoft AAD Application Proxy Connector is an internal domain component that uses outbound connections to establish the connectivity from the cloud available endpoint to the internal domain.</span></span>

## <a name="general-problem-areas-with-connector-installation"></a><span data-ttu-id="73014-105">Allmänna problemområden med kopplingsinstallationen</span><span class="sxs-lookup"><span data-stu-id="73014-105">General Problem Areas with Connector installation</span></span>

<span data-ttu-id="73014-106">När installationen av en anslutning misslyckas är den grundläggande orsaken vanligtvis en av följande områden:</span><span class="sxs-lookup"><span data-stu-id="73014-106">When the installation of a connector fails, the root cause is usually one of the following areas:</span></span>

1.  <span data-ttu-id="73014-107">**Anslutningen** – för att slutföra installationen har lyckats ny koppling behov att registrera och upprätta förtroende för framtida egenskaper.</span><span class="sxs-lookup"><span data-stu-id="73014-107">**Connectivity** – to complete a successful installation, the new connector needs to register and establish future trust properties.</span></span> <span data-ttu-id="73014-108">Detta görs genom att ansluta till Molntjänsten AAD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="73014-108">This is done by connecting to the AAD Application Proxy cloud service.</span></span>

2.  <span data-ttu-id="73014-109">**Upprätta förtroende** – den nya kopplingen skapar ett självsignerat certifikat och registrerar till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="73014-109">**Trust Establishment** – the new connector creates a self-signed cert and registers to the cloud service.</span></span>

3.  <span data-ttu-id="73014-110">**Autentisering av administratören** – under installationen, måste användaren ange administratörsautentiseringsuppgifter för att slutföra installationen av koppling.</span><span class="sxs-lookup"><span data-stu-id="73014-110">**Authentication of the admin** – during installation, the user must provide admin credentials to complete the Connector installation.</span></span>

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a><span data-ttu-id="73014-111">Kontrollera anslutningen till tjänsten för Webbprogramproxy för molnet och Microsoft Login-sida</span><span class="sxs-lookup"><span data-stu-id="73014-111">Verify connectivity to the Cloud Application Proxy service and Microsoft Login page</span></span>

<span data-ttu-id="73014-112">**Mål:** Kontrollera att connector-datorn kan ansluta till slutpunkt för registrering av AAD Application Proxy samt Microsoft inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="73014-112">**Objective:** Verify that the connector machine can connect to the AAD Application Proxy registration endpoint as well as Microsoft login page.</span></span>

1.  <span data-ttu-id="73014-113">Öppna en webbläsare och gå till följande webbsida: <https://aadap-portcheck.connectorporttest.msappproxy.net> , och kontrollera att anslutningen till centrala USA och östra USA datacenter med portarna 9090 och 9091 fungerar.</span><span class="sxs-lookup"><span data-stu-id="73014-113">Open a browser and go to the following web page: <https://aadap-portcheck.connectorporttest.msappproxy.net> , and verify that the connectivity to Central US and East US datacenters with ports 9090 and 9091 is working.</span></span>

2.  <span data-ttu-id="73014-114">Om någon av dessa portar inte lyckas (om du inte har en grön bock), kontrollera att brandväggen eller backend-proxy har \*. msappproxy.net med portarna 9090 och 9091 definierats korrekt.</span><span class="sxs-lookup"><span data-stu-id="73014-114">If any of those ports is not successful (doesn’t have a green checkmark), verify that the Firewall or backend proxy has \*.msappproxy.net with ports 9090 and 9091 defined correctly.</span></span>

3.  <span data-ttu-id="73014-115">Öppna en webbläsare (separat flik) och gå till följande webbsida: <https://login.microsoftonline.com>, se till att du kan logga in på sidan.</span><span class="sxs-lookup"><span data-stu-id="73014-115">Open a browser (separate tab) and go to the following web page: <https://login.microsoftonline.com>, make sure that you can login to that page.</span></span>

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a><span data-ttu-id="73014-116">Kontrollera att dator-och backend-stöd för Application Proxy betrodda certifikat</span><span class="sxs-lookup"><span data-stu-id="73014-116">Verify Machine and backend components support for Application Proxy trust cert</span></span>

<span data-ttu-id="73014-117">**Mål:** Kontrollera att connector-datorn, backend-proxy och brandväggen har stöd för det certifikat som skapas av anslutningen för framtida förtroende.</span><span class="sxs-lookup"><span data-stu-id="73014-117">**Objective:** Verify that the connector machine, backend proxy and firewall can support the certificate created by the connector for future trust.</span></span>

>[!NOTE]
><span data-ttu-id="73014-118">Kopplingen försöker skapa ett SHA512 certifikat som stöds av TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="73014-118">The connector tries to create a SHA512 cert that is supported by TLS1.2.</span></span> <span data-ttu-id="73014-119">Installationen misslyckas om datorn eller backend brandväggar och proxyservrar inte stöder TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="73014-119">If the machine or the backend firewall and proxy does not support TLS1.2, the installation fail.</span></span>
>
>

<span data-ttu-id="73014-120">**Så här löser du problemet:**</span><span class="sxs-lookup"><span data-stu-id="73014-120">**To resolve the issue:**</span></span>

1.  <span data-ttu-id="73014-121">Kontrollera att datorn stöder TLS1.2 – TLS 1.2 ska ha stöd för alla Windows-versioner när 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="73014-121">Verify the machine supports TLS1.2 – All Windows versions after 2012 R2 should support TLS 1.2.</span></span> <span data-ttu-id="73014-122">Om datorn connector är från en version av 2012 R2 eller tidigare, kontrollerar du att följande KBs är installerade på datorn: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span><span class="sxs-lookup"><span data-stu-id="73014-122">If your connector machine is from a version of 2012 R2 or prior, make sure that the following KBs are installed on the machine: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span></span>

2.  <span data-ttu-id="73014-123">Kontakta administratören för nätverket och be att kontrollera att backend proxy- och brandväggsinställningarna inte blockerar SHA512 för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="73014-123">Contact your network admin and ask to verify that the backend proxy and firewall do not block SHA512 for outgoing traffic.</span></span>

## <a name="verify-admin-is-used-to-install-the-connector"></a><span data-ttu-id="73014-124">Kontrollera att admin används för att installera connector</span><span class="sxs-lookup"><span data-stu-id="73014-124">Verify admin is used to install the connector</span></span>

<span data-ttu-id="73014-125">**Mål:** Kontrollera att den användare som försöker installera connector är en administratör med rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="73014-125">**Objective:** Verify that the user who tries to install the connector is an administrator with correct credentials.</span></span> <span data-ttu-id="73014-126">För närvarande måste användaren vara en global administratör för att installationen ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="73014-126">Currently, the user must be a global administrator for the installation to succeed.</span></span>

<span data-ttu-id="73014-127">**Kontrollera att autentiseringsuppgifterna är korrekta:**</span><span class="sxs-lookup"><span data-stu-id="73014-127">**To verify the credentials are correct:**</span></span>

<span data-ttu-id="73014-128">Ansluta till <https://login.microsoftonline.com> och använda samma autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="73014-128">Connect to <https://login.microsoftonline.com> and use the same credentials.</span></span> <span data-ttu-id="73014-129">Kontrollera att inloggningen är klar.</span><span class="sxs-lookup"><span data-stu-id="73014-129">Make sure the login is successful.</span></span> <span data-ttu-id="73014-130">Du kan kontrollera rollen genom att gå till **Azure Active Directory**  - &gt; **användare och grupper**  - &gt; **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="73014-130">You can check the user role by going to **Azure Active Directory** -&gt; **Users and Groups** -&gt; **All Users**.</span></span> 

<span data-ttu-id="73014-131">Välj ditt användarkonto, sedan ”Directory roll” på menyn.</span><span class="sxs-lookup"><span data-stu-id="73014-131">Select your user account, then “Directory Role” in the resulting menu.</span></span> <span data-ttu-id="73014-132">Kontrollera att den valda rollen är ”Global administratör”.</span><span class="sxs-lookup"><span data-stu-id="73014-132">Verify that the selected role is “Global administrator”.</span></span> <span data-ttu-id="73014-133">Om du inte går att komma åt någon av sidorna på de här stegen kan är du inte en global administratör.</span><span class="sxs-lookup"><span data-stu-id="73014-133">If you are unable to access any of the pages along these steps, you are not a global administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73014-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73014-134">Next steps</span></span>
[<span data-ttu-id="73014-135">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="73014-135">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
