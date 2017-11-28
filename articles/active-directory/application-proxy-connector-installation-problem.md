---
title: installera aaaProblem hello Application Proxy Agent Connector | Microsoft Docs
description: "Hur tootroubleshoot problem som du kan hello står inför när du installerar Application Proxy Agent Connector"
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
ms.openlocfilehash: 07ac366a429083af0c9b87aa9df9cf3876132b90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-proxy-agent-connector"></a><span data-ttu-id="f44bc-103">Problem med att installera hello Application Proxy Agent Connector</span><span class="sxs-lookup"><span data-stu-id="f44bc-103">Problem installing hello Application Proxy Agent Connector</span></span>

<span data-ttu-id="f44bc-104">Microsoft AAD Application Proxy Connector är en intern domän-komponent som använder utgående anslutningar tooestablish hello anslutningen från hello molnet tillgängliga endpoint toohello interna domänen.</span><span class="sxs-lookup"><span data-stu-id="f44bc-104">Microsoft AAD Application Proxy Connector is an internal domain component that uses outbound connections tooestablish hello connectivity from hello cloud available endpoint toohello internal domain.</span></span>

## <a name="general-problem-areas-with-connector-installation"></a><span data-ttu-id="f44bc-105">Allmänna problemområden med kopplingsinstallationen</span><span class="sxs-lookup"><span data-stu-id="f44bc-105">General Problem Areas with Connector installation</span></span>

<span data-ttu-id="f44bc-106">När hello installationen av en anslutning misslyckas, är hello orsaken oftast en av hello följande områden:</span><span class="sxs-lookup"><span data-stu-id="f44bc-106">When hello installation of a connector fails, hello root cause is usually one of hello following areas:</span></span>

1.  <span data-ttu-id="f44bc-107">**Anslutningen** – toocomplete en lyckad installation hello nya connector måste tooregister och upprätta förtroende för framtida egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f44bc-107">**Connectivity** – toocomplete a successful installation, hello new connector needs tooregister and establish future trust properties.</span></span> <span data-ttu-id="f44bc-108">Detta görs genom att ansluta toohello AAD Application Proxy-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="f44bc-108">This is done by connecting toohello AAD Application Proxy cloud service.</span></span>

2.  <span data-ttu-id="f44bc-109">**Upprätta förtroende** – hello ny koppling skapar ett självsignerat certifikat och registrerar toohello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="f44bc-109">**Trust Establishment** – hello new connector creates a self-signed cert and registers toohello cloud service.</span></span>

3.  <span data-ttu-id="f44bc-110">**Autentisering av Hej administratör** – under installationen hello måste användaren ange administratörsautentiseringsuppgifter toocomplete hello Connector-installationen.</span><span class="sxs-lookup"><span data-stu-id="f44bc-110">**Authentication of hello admin** – during installation, hello user must provide admin credentials toocomplete hello Connector installation.</span></span>

## <a name="verify-connectivity-toohello-cloud-application-proxy-service-and-microsoft-login-page"></a><span data-ttu-id="f44bc-111">Kontrollera anslutningen toohello moln Application Proxy-tjänsten och Microsoft Login-sida</span><span class="sxs-lookup"><span data-stu-id="f44bc-111">Verify connectivity toohello Cloud Application Proxy service and Microsoft Login page</span></span>

<span data-ttu-id="f44bc-112">**Mål:** verifiera att hello connector datorn kan ansluta toohello AAD Application Proxy registrering slutpunkt som Microsoft-inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="f44bc-112">**Objective:** Verify that hello connector machine can connect toohello AAD Application Proxy registration endpoint as well as Microsoft login page.</span></span>

1.  <span data-ttu-id="f44bc-113">Öppna en webbläsare och gå toohello följande webbsida: <https://aadap-portcheck.connectorporttest.msappproxy.net> , och kontrollera att hello anslutningen tooCentral USA och östra USA datacenter med portarna 9090 och 9091 fungerar.</span><span class="sxs-lookup"><span data-stu-id="f44bc-113">Open a browser and go toohello following web page: <https://aadap-portcheck.connectorporttest.msappproxy.net> , and verify that hello connectivity tooCentral US and East US datacenters with ports 9090 and 9091 is working.</span></span>

2.  <span data-ttu-id="f44bc-114">Om någon av dessa portar inte lyckas (om du inte har en grön bock), verifiera att hello brandväggen eller backend-proxy har \*. msappproxy.net med portarna 9090 och 9091 definierats korrekt.</span><span class="sxs-lookup"><span data-stu-id="f44bc-114">If any of those ports is not successful (doesn’t have a green checkmark), verify that hello Firewall or backend proxy has \*.msappproxy.net with ports 9090 and 9091 defined correctly.</span></span>

3.  <span data-ttu-id="f44bc-115">Öppna en webbläsare (separat flik) och gå toohello följande webbsida: <https://login.microsoftonline.com>, se till att du loggar in toothat sidan.</span><span class="sxs-lookup"><span data-stu-id="f44bc-115">Open a browser (separate tab) and go toohello following web page: <https://login.microsoftonline.com>, make sure that you can login toothat page.</span></span>

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a><span data-ttu-id="f44bc-116">Kontrollera att dator-och backend-stöd för Application Proxy betrodda certifikat</span><span class="sxs-lookup"><span data-stu-id="f44bc-116">Verify Machine and backend components support for Application Proxy trust cert</span></span>

<span data-ttu-id="f44bc-117">**Mål:** Kontrollera att hello connector datorn, backend-proxy och brandväggen stöder hello-certifikat som skapas av hello connector för framtida förtroende.</span><span class="sxs-lookup"><span data-stu-id="f44bc-117">**Objective:** Verify that hello connector machine, backend proxy and firewall can support hello certificate created by hello connector for future trust.</span></span>

>[!NOTE]
><span data-ttu-id="f44bc-118">hello-koppling försöker toocreate ett SHA512 certifikat som stöds av TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="f44bc-118">hello connector tries toocreate a SHA512 cert that is supported by TLS1.2.</span></span> <span data-ttu-id="f44bc-119">Om datorn hello eller hello backend brandväggar och proxyservrar inte stöder TLS1.2 hello installationen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="f44bc-119">If hello machine or hello backend firewall and proxy does not support TLS1.2, hello installation fail.</span></span>
>
>

<span data-ttu-id="f44bc-120">**tooresolve hello problem:**</span><span class="sxs-lookup"><span data-stu-id="f44bc-120">**tooresolve hello issue:**</span></span>

1.  <span data-ttu-id="f44bc-121">Kontrollera hello datorn stöder TLS1.2 – TLS 1.2 ska ha stöd för alla Windows-versioner när 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="f44bc-121">Verify hello machine supports TLS1.2 – All Windows versions after 2012 R2 should support TLS 1.2.</span></span> <span data-ttu-id="f44bc-122">Om datorn connector är från en version av 2012 R2 eller tidigare, kontrollerar du att hello följande KBs är installerade på datorn hello: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span><span class="sxs-lookup"><span data-stu-id="f44bc-122">If your connector machine is from a version of 2012 R2 or prior, make sure that hello following KBs are installed on hello machine: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span></span>

2.  <span data-ttu-id="f44bc-123">Kontakta administratören för nätverket och be tooverify att hello backend proxy- och brandväggsinställningarna inte blockerar SHA512 för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="f44bc-123">Contact your network admin and ask tooverify that hello backend proxy and firewall do not block SHA512 for outgoing traffic.</span></span>

## <a name="verify-admin-is-used-tooinstall-hello-connector"></a><span data-ttu-id="f44bc-124">Kontrollera admin används tooinstall hello-koppling</span><span class="sxs-lookup"><span data-stu-id="f44bc-124">Verify admin is used tooinstall hello connector</span></span>

<span data-ttu-id="f44bc-125">**Mål:** kontrollerar du att hello-användaren som försöker tooinstall hello connector är en administratör med rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f44bc-125">**Objective:** Verify that hello user who tries tooinstall hello connector is an administrator with correct credentials.</span></span> <span data-ttu-id="f44bc-126">För närvarande måste hello användaren vara en global administratör för hello installation toosucceed.</span><span class="sxs-lookup"><span data-stu-id="f44bc-126">Currently, hello user must be a global administrator for hello installation toosucceed.</span></span>

<span data-ttu-id="f44bc-127">**tooverify hello autentiseringsuppgifterna är korrekta:**</span><span class="sxs-lookup"><span data-stu-id="f44bc-127">**tooverify hello credentials are correct:**</span></span>

<span data-ttu-id="f44bc-128">Ansluta för<https://login.microsoftonline.com> och använda hello samma autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f44bc-128">Connect too<https://login.microsoftonline.com> and use hello same credentials.</span></span> <span data-ttu-id="f44bc-129">Kontrollera att hello inloggningen är klar.</span><span class="sxs-lookup"><span data-stu-id="f44bc-129">Make sure hello login is successful.</span></span> <span data-ttu-id="f44bc-130">Du kan kontrollera hello användarroll genom att gå för**Azure Active Directory**  - &gt; **användare och grupper**  - &gt; **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f44bc-130">You can check hello user role by going too**Azure Active Directory** -&gt; **Users and Groups** -&gt; **All Users**.</span></span> 

<span data-ttu-id="f44bc-131">Välj ditt användarkonto, sedan ”Directory roll” hello resulterande menyn.</span><span class="sxs-lookup"><span data-stu-id="f44bc-131">Select your user account, then “Directory Role” in hello resulting menu.</span></span> <span data-ttu-id="f44bc-132">Verifiera den valda rollen hello är ”Global administratör”.</span><span class="sxs-lookup"><span data-stu-id="f44bc-132">Verify that hello selected role is “Global administrator”.</span></span> <span data-ttu-id="f44bc-133">Om du tooaccess någon hello sidor längs de här stegen kan är du inte en global administratör.</span><span class="sxs-lookup"><span data-stu-id="f44bc-133">If you are unable tooaccess any of hello pages along these steps, you are not a global administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f44bc-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f44bc-134">Next steps</span></span>
[<span data-ttu-id="f44bc-135">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="f44bc-135">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
