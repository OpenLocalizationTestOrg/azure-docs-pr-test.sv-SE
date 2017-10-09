---
title: aaaMicrosoft hot Modeling verktyget - Azure | Microsoft Docs
description: "Lär dig mer om alla hello-funktionerna i hello hot Modeling verktyget"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: f9ad5e623e7758063084cb7fc723c5735161a846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="threat-modeling-tool-feature-overview"></a><span data-ttu-id="da4b6-103">Översikt över hot Modeling verktyget funktioner</span><span class="sxs-lookup"><span data-stu-id="da4b6-103">Threat Modeling Tool feature overview</span></span>

<span data-ttu-id="da4b6-104">Vi är glada över att du har valt toouse hello hot Modeling verktyget för din hot modeling behov!</span><span class="sxs-lookup"><span data-stu-id="da4b6-104">We are glad you chose toouse hello Threat Modeling Tool for your threat modeling needs!</span></span> <span data-ttu-id="da4b6-105">Om du inte gjort det, gå  **[komma igång med hello hot Modeling verktyget](./azure-security-threat-modeling-tool-getting-started.md)**  toolearn hello grunderna.</span><span class="sxs-lookup"><span data-stu-id="da4b6-105">If you haven’t done so, visit **[Getting Started with hello Threat Modeling Tool](./azure-security-threat-modeling-tool-getting-started.md)** toolearn hello basics.</span></span>

> <span data-ttu-id="da4b6-106">Vårt verktyg uppdateras ofta, så kontrollera detta leder ofta toosee vår nya funktioner och korrigeringar.</span><span class="sxs-lookup"><span data-stu-id="da4b6-106">Our tool is updated often, so check this guide often toosee our latest features and improvements.</span></span>

<span data-ttu-id="da4b6-107">Klicka på ”Skapa en ny modell” hello öppnas en tom startsida, liknande toohello bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="da4b6-107">Clicking on hello "Create a New Model" button opens a blank start page, similar toohello image below:</span></span>

![Tom startsida](./media/azure-security-threat-modeling-tool/tmtstart.png)

<span data-ttu-id="da4b6-109">Använda hello hotmodell som skapats av vårt team i hello  **[komma igång](./azure-security-threat-modeling-tool-getting-started.md)**  exemplet ska vi ta en titt alla hello-funktionerna i hello verktyget idag.</span><span class="sxs-lookup"><span data-stu-id="da4b6-109">Using hello threat model created by our team in hello **[Getting Started](./azure-security-threat-modeling-tool-getting-started.md)** example, let’s check out all hello features available in hello tool today.</span></span>

![Grundläggande Hotmodell](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a><span data-ttu-id="da4b6-111">Navigering</span><span class="sxs-lookup"><span data-stu-id="da4b6-111">Navigation</span></span>

<span data-ttu-id="da4b6-112">Innan du dyker in hello inbyggda funktioner ska vi gå igenom hello huvudkomponenterna i hello-verktyget</span><span class="sxs-lookup"><span data-stu-id="da4b6-112">Before diving into hello built-in features, let’s go over hello main components found in hello tool</span></span>

### <a name="menu-items"></a><span data-ttu-id="da4b6-113">Menyalternativ</span><span class="sxs-lookup"><span data-stu-id="da4b6-113">Menu items</span></span>

<span data-ttu-id="da4b6-114">hello vara liknande tooother Microsoft-produkter.</span><span class="sxs-lookup"><span data-stu-id="da4b6-114">hello experience should be similar tooother Microsoft products.</span></span> <span data-ttu-id="da4b6-115">Vi börjar med att gå igenom hello menyobjekt:</span><span class="sxs-lookup"><span data-stu-id="da4b6-115">Let’s begin by going through hello top-level menu items:</span></span>

![Menyalternativ](./media/azure-security-threat-modeling-tool/menuitems.png)

| <span data-ttu-id="da4b6-117">Etikett</span><span class="sxs-lookup"><span data-stu-id="da4b6-117">Label</span></span>                               | <span data-ttu-id="da4b6-118">Information</span><span class="sxs-lookup"><span data-stu-id="da4b6-118">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="da4b6-119">**Fil**</span><span class="sxs-lookup"><span data-stu-id="da4b6-119">**File**</span></span> | <ul><li><span data-ttu-id="da4b6-120">Öppna, spara och stänga filer</span><span class="sxs-lookup"><span data-stu-id="da4b6-120">Open, Save and Close Files</span></span></li><li><span data-ttu-id="da4b6-121">Logga in/ut av OneDrive konton</span><span class="sxs-lookup"><span data-stu-id="da4b6-121">Sign In/Out of OneDrive accounts</span></span></li><li><span data-ttu-id="da4b6-122">Dela länkar (visa + redigera)</span><span class="sxs-lookup"><span data-stu-id="da4b6-122">Share Links (View + Edit)</span></span></li><li><span data-ttu-id="da4b6-123">Visa filinformation</span><span class="sxs-lookup"><span data-stu-id="da4b6-123">View File Information</span></span></li><li><span data-ttu-id="da4b6-124">Tillämpa mall tooExisting modeller</span><span class="sxs-lookup"><span data-stu-id="da4b6-124">Apply New Template tooExisting Models</span></span></li></ul> |
| <span data-ttu-id="da4b6-125">**Redigera**</span><span class="sxs-lookup"><span data-stu-id="da4b6-125">**Edit**</span></span> | <span data-ttu-id="da4b6-126">Ångra/Gör om åtgärder, som även en kopia, klistra in och ta bort</span><span class="sxs-lookup"><span data-stu-id="da4b6-126">Undo/redo actions, as well a copy, paste and delete</span></span> |
| <span data-ttu-id="da4b6-127">**Visa**</span><span class="sxs-lookup"><span data-stu-id="da4b6-127">**View**</span></span> | <ul><li><span data-ttu-id="da4b6-128">Växla mellan **Analysis** och **Design** vyer</span><span class="sxs-lookup"><span data-stu-id="da4b6-128">Switch between **Analysis** and **Design** views</span></span></li><li><span data-ttu-id="da4b6-129">Öppna stängd windows (e.g.stencils, egenskaper och meddelanden)</span><span class="sxs-lookup"><span data-stu-id="da4b6-129">Open closed windows (e.g.stencils, element properties and messages)</span></span></li><li><span data-ttu-id="da4b6-130">Återställ toodefault layoutinställningarna</span><span class="sxs-lookup"><span data-stu-id="da4b6-130">Reset layout toodefault settings</span></span></li></ul> |
| <span data-ttu-id="da4b6-131">**Diagram**</span><span class="sxs-lookup"><span data-stu-id="da4b6-131">**Diagram**</span></span> | <span data-ttu-id="da4b6-132">Lägg till/ta bort diagram och navigera genom ”flikar” av diagram</span><span class="sxs-lookup"><span data-stu-id="da4b6-132">Add/Delete diagrams and navigate through “tabs” of diagrams</span></span> |
| <span data-ttu-id="da4b6-133">**Rapporter**</span><span class="sxs-lookup"><span data-stu-id="da4b6-133">**Reports**</span></span> | <span data-ttu-id="da4b6-134">Skapa HTML-rapporter tooshare med andra</span><span class="sxs-lookup"><span data-stu-id="da4b6-134">Create HTML reports tooshare with others</span></span> |
| <span data-ttu-id="da4b6-135">**Hjälp**</span><span class="sxs-lookup"><span data-stu-id="da4b6-135">**Help**</span></span> | <span data-ttu-id="da4b6-136">Guider toohelp hello verktyget</span><span class="sxs-lookup"><span data-stu-id="da4b6-136">Guides toohelp you use hello tool</span></span> |

<span data-ttu-id="da4b6-137">hello ikoner är genvägar för hello översta menyer:</span><span class="sxs-lookup"><span data-stu-id="da4b6-137">hello icons are shortcuts for hello top-level menus:</span></span>

| <span data-ttu-id="da4b6-138">Ikon</span><span class="sxs-lookup"><span data-stu-id="da4b6-138">Icon</span></span>                               | <span data-ttu-id="da4b6-139">Information</span><span class="sxs-lookup"><span data-stu-id="da4b6-139">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="da4b6-140">**Öppna**</span><span class="sxs-lookup"><span data-stu-id="da4b6-140">**Open**</span></span> | <span data-ttu-id="da4b6-141">Öppnar en ny fil</span><span class="sxs-lookup"><span data-stu-id="da4b6-141">Opens a new file</span></span> |
| <span data-ttu-id="da4b6-142">**Spara**</span><span class="sxs-lookup"><span data-stu-id="da4b6-142">**Save**</span></span> | <span data-ttu-id="da4b6-143">Sparar aktuell fil</span><span class="sxs-lookup"><span data-stu-id="da4b6-143">Saves current file</span></span> |
| <span data-ttu-id="da4b6-144">**Design**</span><span class="sxs-lookup"><span data-stu-id="da4b6-144">**Design**</span></span> | <span data-ttu-id="da4b6-145">Försätts i designvyn, där du kan skapa modeller</span><span class="sxs-lookup"><span data-stu-id="da4b6-145">Goes into design view, where you can create models</span></span> |
| <span data-ttu-id="da4b6-146">**Analysera**</span><span class="sxs-lookup"><span data-stu-id="da4b6-146">**Analyze**</span></span> | <span data-ttu-id="da4b6-147">Visar genererade hot och deras egenskaper</span><span class="sxs-lookup"><span data-stu-id="da4b6-147">Shows generated threats and their properties</span></span> |
| <span data-ttu-id="da4b6-148">**Lägg till Diagram**</span><span class="sxs-lookup"><span data-stu-id="da4b6-148">**Add Diagram**</span></span> | <span data-ttu-id="da4b6-149">Lägger till nya diagram (liknande toonew flikar i Excel)</span><span class="sxs-lookup"><span data-stu-id="da4b6-149">Adds new diagram (similar toonew tabs in Excel)</span></span> |
| <span data-ttu-id="da4b6-150">**Ta bort Diagram**</span><span class="sxs-lookup"><span data-stu-id="da4b6-150">**Delete Diagram**</span></span> | <span data-ttu-id="da4b6-151">Tar bort aktuella diagrammet</span><span class="sxs-lookup"><span data-stu-id="da4b6-151">Deletes current diagram</span></span> |
| <span data-ttu-id="da4b6-152">**Kopiera och klipp ut/klistra in**</span><span class="sxs-lookup"><span data-stu-id="da4b6-152">**Copy/Cut/Paste**</span></span> | <span data-ttu-id="da4b6-153">Klistrar in-kopior/delar element</span><span class="sxs-lookup"><span data-stu-id="da4b6-153">Copies/cuts/pastes elements</span></span> |
| <span data-ttu-id="da4b6-154">**Ångra/Gör om**</span><span class="sxs-lookup"><span data-stu-id="da4b6-154">**Undo/Redo**</span></span> | <span data-ttu-id="da4b6-155">Ångrar/gör om åtgärder</span><span class="sxs-lookup"><span data-stu-id="da4b6-155">Undoes/redoes actions</span></span> |
| <span data-ttu-id="da4b6-156">**Zooma In / Zooma ut**</span><span class="sxs-lookup"><span data-stu-id="da4b6-156">**Zoom In/Zoom Out**</span></span> | <span data-ttu-id="da4b6-157">Zoomar in och ut hello diagram för en bättre vy</span><span class="sxs-lookup"><span data-stu-id="da4b6-157">Zooms in and out of hello diagram for a better view</span></span> |
| <span data-ttu-id="da4b6-158">**Feedback**</span><span class="sxs-lookup"><span data-stu-id="da4b6-158">**Feedback**</span></span> | <span data-ttu-id="da4b6-159">Öppnar hello MSDN-Forum</span><span class="sxs-lookup"><span data-stu-id="da4b6-159">Opens hello MSDN Forum</span></span> |

### <a name="canvas"></a><span data-ttu-id="da4b6-160">Arbetsytan</span><span class="sxs-lookup"><span data-stu-id="da4b6-160">Canvas</span></span>

<span data-ttu-id="da4b6-161">hello diskutrymme där du drar och släpper element i.</span><span class="sxs-lookup"><span data-stu-id="da4b6-161">hello space where you drag and drop elements into.</span></span> <span data-ttu-id="da4b6-162">Dra och släpp är hello snabbaste och mest effektiva sättet toobuild modeller.</span><span class="sxs-lookup"><span data-stu-id="da4b6-162">Drag and drop is hello quickest and most efficient way toobuild models.</span></span> <span data-ttu-id="da4b6-163">Du kan också högerklicka och välj hello-menyn, som lägger till allmän versioner av hello-element som du använder, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="da4b6-163">You may also right click and select from hello menu, which adds generic versions of hello elements you’re using, as shown below.</span></span>

#### <a name="dropping-hello-stencil-on-hello-canvas"></a><span data-ttu-id="da4b6-164">Släppa hello stencil på hello arbetsytan</span><span class="sxs-lookup"><span data-stu-id="da4b6-164">Dropping hello stencil on hello canvas</span></span>

![Arbetsytan släpp](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-hello-stencil"></a><span data-ttu-id="da4b6-166">Klicka på hello stencil</span><span class="sxs-lookup"><span data-stu-id="da4b6-166">Clicking on hello stencil</span></span>

![Egenskaper](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a><span data-ttu-id="da4b6-168">Stenciler</span><span class="sxs-lookup"><span data-stu-id="da4b6-168">Stencils</span></span>

<span data-ttu-id="da4b6-169">Om du hittar alla stenciler tillgängliga toouse baserat på valda hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="da4b6-169">Where you can find all stencils available toouse based on hello template selected.</span></span> <span data-ttu-id="da4b6-170">Om du inte hittar rätt hello-element, försök med en annan mall eller ändra en toofit dina behov.</span><span class="sxs-lookup"><span data-stu-id="da4b6-170">If you can’t find hello right elements, try using another template, or modify one toofit your needs.</span></span> <span data-ttu-id="da4b6-171">I allmänhet bör du vara kan toofind en kombination av kategorier som hello de nedan:</span><span class="sxs-lookup"><span data-stu-id="da4b6-171">Generally, you should be able toofind a combination of categories like hello ones below:</span></span>

| <span data-ttu-id="da4b6-172">Stencilens namn</span><span class="sxs-lookup"><span data-stu-id="da4b6-172">Stencil Name</span></span>                               | <span data-ttu-id="da4b6-173">Information</span><span class="sxs-lookup"><span data-stu-id="da4b6-173">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="da4b6-174">**Processen**</span><span class="sxs-lookup"><span data-stu-id="da4b6-174">**Process**</span></span> | <span data-ttu-id="da4b6-175">Program, webbläsare plugin-program, trådar, virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="da4b6-175">Applications, Browser Plugins, Threads, Virtual Machines</span></span> |
| <span data-ttu-id="da4b6-176">**Extern kontakt**</span><span class="sxs-lookup"><span data-stu-id="da4b6-176">**External Interactor**</span></span> | <span data-ttu-id="da4b6-177">Autentiseringsproviders, webbläsare, användare, webbprogram</span><span class="sxs-lookup"><span data-stu-id="da4b6-177">Authentication Providers, Browsers, Users, Web Applications</span></span> |
| <span data-ttu-id="da4b6-178">**Datalager**</span><span class="sxs-lookup"><span data-stu-id="da4b6-178">**Data Store**</span></span> | <span data-ttu-id="da4b6-179">Cache, lagring, Configuration-filer, databaser, registret</span><span class="sxs-lookup"><span data-stu-id="da4b6-179">Cache, Storage, Configuration Files, Databases, Registry</span></span> |
| <span data-ttu-id="da4b6-180">**Dataflöde**</span><span class="sxs-lookup"><span data-stu-id="da4b6-180">**Data Flow**</span></span> | <span data-ttu-id="da4b6-181">Binär, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, namngivna Pipe, RPC/DCOM, SMB, UDP</span><span class="sxs-lookup"><span data-stu-id="da4b6-181">Binary, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP</span></span> |
| <span data-ttu-id="da4b6-182">**Förtroende kantlinje/gräns**</span><span class="sxs-lookup"><span data-stu-id="da4b6-182">**Trust Line/Border Boundary**</span></span> | <span data-ttu-id="da4b6-183">Företagets nätverk, Internet, dator, Sandbox, användar-/ Kernel-läge</span><span class="sxs-lookup"><span data-stu-id="da4b6-183">Corporate Networks, Internet, Machine, Sandbox, User/Kernel Mode</span></span> |

### <a name="notesmessages"></a><span data-ttu-id="da4b6-184">Anteckningar/meddelanden</span><span class="sxs-lookup"><span data-stu-id="da4b6-184">Notes/Messages</span></span>

| <span data-ttu-id="da4b6-185">Komponent</span><span class="sxs-lookup"><span data-stu-id="da4b6-185">Component</span></span>                               | <span data-ttu-id="da4b6-186">Information</span><span class="sxs-lookup"><span data-stu-id="da4b6-186">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="da4b6-187">**Meddelanden**</span><span class="sxs-lookup"><span data-stu-id="da4b6-187">**Messages**</span></span> | <span data-ttu-id="da4b6-188">Interna verktyget logik som varnar användare när det uppstår ett fel, till exempel inga data som flödar mellan element</span><span class="sxs-lookup"><span data-stu-id="da4b6-188">Internal tool logic that alerts users whenever there is an error, such as no data flows between elements</span></span> |
| <span data-ttu-id="da4b6-189">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="da4b6-189">**Notes**</span></span> | <span data-ttu-id="da4b6-190">Manuell anteckningar tillagda toohello filen med engineering team hela hello design och granska process</span><span class="sxs-lookup"><span data-stu-id="da4b6-190">Manual notes added toohello file by engineering teams throughout hello design and review process</span></span> |

### <a name="element-properties"></a><span data-ttu-id="da4b6-191">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="da4b6-191">Element properties</span></span>

<span data-ttu-id="da4b6-192">Dessa varierar beroende på hello-element som valts.</span><span class="sxs-lookup"><span data-stu-id="da4b6-192">These vary by hello elements selected.</span></span> <span data-ttu-id="da4b6-193">Förutom förtroende gränser kan innehålla alla element 3 allmänna inställningar:</span><span class="sxs-lookup"><span data-stu-id="da4b6-193">Apart from Trust Boundaries, all other elements contain 3 general selections:</span></span>

| <span data-ttu-id="da4b6-194">Egenskapen element</span><span class="sxs-lookup"><span data-stu-id="da4b6-194">Element Property</span></span>                               | <span data-ttu-id="da4b6-195">Information</span><span class="sxs-lookup"><span data-stu-id="da4b6-195">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="da4b6-196">**Namn**</span><span class="sxs-lookup"><span data-stu-id="da4b6-196">**Name**</span></span> | <span data-ttu-id="da4b6-197">Användbar för att namnge din processer, interaktörer och andra flöden toobe som är lätt att känna igen</span><span class="sxs-lookup"><span data-stu-id="da4b6-197">Useful for naming your processes, stores, interactors and flows toobe easily recognized</span></span> |
| <span data-ttu-id="da4b6-198">**Utanför omfånget**</span><span class="sxs-lookup"><span data-stu-id="da4b6-198">**Out of Scope**</span></span> | <span data-ttu-id="da4b6-199">Om markerad tas hello element ur hello hot generation matris (rekommenderas inte)</span><span class="sxs-lookup"><span data-stu-id="da4b6-199">If selected, hello element is taken out of hello threat generation matrix (not recommended)</span></span> |
| <span data-ttu-id="da4b6-200">**Orsak till utanför omfånget**</span><span class="sxs-lookup"><span data-stu-id="da4b6-200">**Reason for Out of Scope**</span></span> | <span data-ttu-id="da4b6-201">Motiveringen fältet toolet användarna vet varför täcker har valts</span><span class="sxs-lookup"><span data-stu-id="da4b6-201">Justification field toolet users know why out of scope was selected</span></span> |

<span data-ttu-id="da4b6-202">Egenskaper har ändrats under varje element.</span><span class="sxs-lookup"><span data-stu-id="da4b6-202">Properties are changed under each element category.</span></span> <span data-ttu-id="da4b6-203">Klicka på varje element tooinspect hello tillgängliga alternativ eller öppna hello mallen toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="da4b6-203">Click on each element tooinspect hello available options, or open hello template toolearn more.</span></span> <span data-ttu-id="da4b6-204">Nu ska vi gå in hello funktioner.</span><span class="sxs-lookup"><span data-stu-id="da4b6-204">Let’s get into hello features.</span></span>

## <a name="welcome-screen"></a><span data-ttu-id="da4b6-205">Välkomstskärmen</span><span class="sxs-lookup"><span data-stu-id="da4b6-205">Welcome screen</span></span>

<span data-ttu-id="da4b6-206">hello välkomstskärmen är hello första som visas när du öppnar hello app.</span><span class="sxs-lookup"><span data-stu-id="da4b6-206">hello welcome screen is hello first thing you see when you open hello app.</span></span>

### <a name="open-a-model"></a><span data-ttu-id="da4b6-207">Öppna en modell</span><span class="sxs-lookup"><span data-stu-id="da4b6-207">Open A model</span></span>

<span data-ttu-id="da4b6-208">Hovra över knappen ”Öppna en modell” visar 2 dolda alternativ: ”Öppna från den här datorn” och ”öppna från OneDrive”.</span><span class="sxs-lookup"><span data-stu-id="da4b6-208">Hovering over “Open a Model” button shows you 2 hidden options: “Open From this Computer” and “Open from OneDrive.”</span></span> <span data-ttu-id="da4b6-209">hello öppnas först öppna hello-skärmen, medan hello andra tar dig igenom hello logga i processen för OneDrive, så att du toopick mappar och filer efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="da4b6-209">hello first opens hello File Open screen, while hello second takes you through hello sign in process for OneDrive, allowing you toopick folders and files after a successful authentication.</span></span>

![Öppna modellen](./media/azure-security-threat-modeling-tool/openmodel.png)

![Öppna från datorn eller OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a><span data-ttu-id="da4b6-212">Feedback, förslag och problem</span><span class="sxs-lookup"><span data-stu-id="da4b6-212">Feedback, suggestions and issues</span></span>

<span data-ttu-id="da4b6-213">Det här alternativet tar du toohello MSDN-forum för SDL-verktyg.</span><span class="sxs-lookup"><span data-stu-id="da4b6-213">Selecting this option will take you toohello MSDN Forums for SDL Tools.</span></span> <span data-ttu-id="da4b6-214">Det är ett bra sätt toocheck reda på vad andra säger om hello verktyget, inklusive lösningar och nya idéer.</span><span class="sxs-lookup"><span data-stu-id="da4b6-214">It’s a great way toocheck out what other people are saying about hello tool, including workarounds and new ideas.</span></span>

![Feedback](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a><span data-ttu-id="da4b6-216">Designvyn</span><span class="sxs-lookup"><span data-stu-id="da4b6-216">Design view</span></span>

<span data-ttu-id="da4b6-217">När du öppnar eller skapa en ny modell, vidarebefordras du toohello designvyn.</span><span class="sxs-lookup"><span data-stu-id="da4b6-217">Whenever you open or create a new model, you’ll be taken toohello design view.</span></span>

### <a name="adding-elements"></a><span data-ttu-id="da4b6-218">Att lägga till element</span><span class="sxs-lookup"><span data-stu-id="da4b6-218">Adding elements</span></span>

<span data-ttu-id="da4b6-219">Det finns 2 sätt tooadd element i hello rutnät:</span><span class="sxs-lookup"><span data-stu-id="da4b6-219">There are 2 ways tooadd elements on hello grid:</span></span>

- <span data-ttu-id="da4b6-220">**Dra och släpp** – dra hello önskade elementet toohello rutnätet och sedan använda hello elementet egenskaper tooprovide ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="da4b6-220">**Drag and Drop** – drag hello desired element toohello grid, then use hello element properties tooprovide additional information.</span></span>
- <span data-ttu-id="da4b6-221">**Högerklicka på** – högerklickar du någonstans på hello rutnätet och välja från hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="da4b6-221">**Right Click** – right click anywhere on hello grid and select from hello dropdown menu.</span></span> <span data-ttu-id="da4b6-222">En allmän representation av elementet visas på hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="da4b6-222">A generic representation of that element will appear on hello screen.</span></span>

### <a name="connecting-elements"></a><span data-ttu-id="da4b6-223">Ansluta element</span><span class="sxs-lookup"><span data-stu-id="da4b6-223">Connecting elements</span></span>

<span data-ttu-id="da4b6-224">Det finns 2 sätt tooconnect element i hello-verktyget:</span><span class="sxs-lookup"><span data-stu-id="da4b6-224">There are 2 ways tooconnect elements in hello tool:</span></span>

- <span data-ttu-id="da4b6-225">**Dra och släpp** – dra hello önskade dataflöde toohello rutnätet och ansluta båda ändarna toohello lämplig elementen.</span><span class="sxs-lookup"><span data-stu-id="da4b6-225">**Drag and Drop** – drag hello desired dataflow toohello grid, and connect both ends toohello appropriate elements.</span></span>
- <span data-ttu-id="da4b6-226">**Klicka på + SKIFT** – klickar du på hello första elementet (skicka data), tryck och håll hello SKIFT-tangenten och sedan väljer hello andra elementet (mottagning av data).</span><span class="sxs-lookup"><span data-stu-id="da4b6-226">**Click + Shift** – click on hello first element (sending data), press and hold hello Shift key, then select hello second element (receiving data).</span></span> <span data-ttu-id="da4b6-227">Högerklicka och Välj ”Anslut”.</span><span class="sxs-lookup"><span data-stu-id="da4b6-227">Right click, and select “Connect.”</span></span> <span data-ttu-id="da4b6-228">Om du använder en dubbelriktad dataflöde är hello ordningen inte lika viktigt.</span><span class="sxs-lookup"><span data-stu-id="da4b6-228">If you’re using a bi-directional dataflow, hello order is not as important.</span></span>

### <a name="properties"></a><span data-ttu-id="da4b6-229">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="da4b6-229">Properties</span></span>

<span data-ttu-id="da4b6-230">Visar alla hello-egenskaper som kan ändras i hello stenciler placeras i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="da4b6-230">Shows all hello properties that can be modified on hello stencils placed in hello diagram.</span></span> <span data-ttu-id="da4b6-231">toosee hello egenskaper, klicka på hello stencil och hello information fylls i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="da4b6-231">toosee hello properties, just click on hello stencil and hello information will be populated accordingly.</span></span> <span data-ttu-id="da4b6-232">hello exemplet nedan visar före och efter ett ”databas” stencil dras till hello diagram:</span><span class="sxs-lookup"><span data-stu-id="da4b6-232">hello example below shows before and after a "Database" stencil is dragged onto hello diagram:</span></span>

#### <a name="before"></a><span data-ttu-id="da4b6-233">Innan du</span><span class="sxs-lookup"><span data-stu-id="da4b6-233">Before</span></span>

![Innan du](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a><span data-ttu-id="da4b6-235">Efter</span><span class="sxs-lookup"><span data-stu-id="da4b6-235">After</span></span>

![Efter](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a><span data-ttu-id="da4b6-237">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="da4b6-237">Messages</span></span>

<span data-ttu-id="da4b6-238">Om du skapar en hotmodell och glömmer tooconnect data flödar tooelements meddelar hello meddelandefönstret tooact.</span><span class="sxs-lookup"><span data-stu-id="da4b6-238">If you create a threat model and forget tooconnect data flows tooelements, hello message window notifies you tooact.</span></span> <span data-ttu-id="da4b6-239">Du kan välja tooignore den eller följ hello instruktioner toofix hello problemet.</span><span class="sxs-lookup"><span data-stu-id="da4b6-239">You can choose tooignore it or follow hello instructions toofix hello issue.</span></span> 

![Meddelanden](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a><span data-ttu-id="da4b6-241">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="da4b6-241">Notes</span></span>

<span data-ttu-id="da4b6-242">Växla flikar från meddelanden tooNotes tillåter du tooadd anteckningar tooyour diagram toocapture dina tankar</span><span class="sxs-lookup"><span data-stu-id="da4b6-242">Switching tabs from Messages tooNotes allows you tooadd notes tooyour diagram toocapture all your thoughts</span></span>

## <a name="analysis-view"></a><span data-ttu-id="da4b6-243">Analysen</span><span class="sxs-lookup"><span data-stu-id="da4b6-243">Analysis view</span></span>

<span data-ttu-id="da4b6-244">När du är klar skapar diagrammet växla över tooanalysis vyn genom att gå toohello översta menyn Inställningar och välja hello förstoringsglas nästa toohello paint palett.</span><span class="sxs-lookup"><span data-stu-id="da4b6-244">Once you're done building your diagram, switch over tooanalysis view by going toohello top menu selections and choosing hello magnifying glass next toohello paint palette.</span></span>

![Analysen](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a><span data-ttu-id="da4b6-246">Val av genererade hot</span><span class="sxs-lookup"><span data-stu-id="da4b6-246">Generated threat selection</span></span>

<span data-ttu-id="da4b6-247">När du klickar på ett hot kan du utnyttja tre unika funktioner:</span><span class="sxs-lookup"><span data-stu-id="da4b6-247">When you click on a threat, you can leverage three unique functions:</span></span>

| <span data-ttu-id="da4b6-248">Funktion</span><span class="sxs-lookup"><span data-stu-id="da4b6-248">Feature</span></span>                               | <span data-ttu-id="da4b6-249">Information</span><span class="sxs-lookup"><span data-stu-id="da4b6-249">Info</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="da4b6-250">**Läs indikator**</span><span class="sxs-lookup"><span data-stu-id="da4b6-250">**Read Indicator**</span></span> | <p><span data-ttu-id="da4b6-251">Hot har nu markerats som läst som enkelt kan hjälpa dig att hålla reda på hello-objekt som du redan har gått igenom</span><span class="sxs-lookup"><span data-stu-id="da4b6-251">Threat is now marked as read, which can easily help you keep track of hello items you already went through</span></span></p><p>![Läs oläst indikator](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| <span data-ttu-id="da4b6-253">**Interaktion fokus**</span><span class="sxs-lookup"><span data-stu-id="da4b6-253">**Interaction Focus**</span></span> | <p><span data-ttu-id="da4b6-254">Interaktion i hello diagram tillhörande toothat hot är markerat</span><span class="sxs-lookup"><span data-stu-id="da4b6-254">Interaction in hello diagram belonging toothat threat is highlighted</span></span></p><p>![Interaktion fokus](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| <span data-ttu-id="da4b6-256">**Egenskaper för hot**</span><span class="sxs-lookup"><span data-stu-id="da4b6-256">**Threat Properties**</span></span> | <p><span data-ttu-id="da4b6-257">Mer information om hello hot fylls i egenskapsfönstret för hello hot</span><span class="sxs-lookup"><span data-stu-id="da4b6-257">Additional information about hello threat is populated in hello threat properties window</span></span></p><p>![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a><span data-ttu-id="da4b6-259">Ändra prioritet</span><span class="sxs-lookup"><span data-stu-id="da4b6-259">Priority change</span></span>

<span data-ttu-id="da4b6-260">Ändrar hello prioritetsnivån för varje genererade hot ändras deras färger toomake den enkelt tooidentify hög, medel och låg prioritet hot.</span><span class="sxs-lookup"><span data-stu-id="da4b6-260">Changing hello priority level of each generated threat also changes their colors toomake it easy tooidentify high, medium and low priority threats.</span></span>

![Ändra prioritet](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a><span data-ttu-id="da4b6-262">Redigerbart fält för hot egenskaper</span><span class="sxs-lookup"><span data-stu-id="da4b6-262">Threat properties editable fields</span></span>

<span data-ttu-id="da4b6-263">Som det visas i hello bilden ovan, kan användare ändra hello information som genererats av hello verktyget en också lägga till information toocertain fält, till exempel justering.</span><span class="sxs-lookup"><span data-stu-id="da4b6-263">As seen in hello image above, users can change hello information generated by hello tool an also add information toocertain fields, such as justification.</span></span> <span data-ttu-id="da4b6-264">Fälten genereras av hello mallen så att om du behöver mer information för varje hot du rekommenderar toomake ändringar.</span><span class="sxs-lookup"><span data-stu-id="da4b6-264">These fields are generated by hello template, so if you need more information for each threat, you're encouraged toomake modifications.</span></span>

![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a><span data-ttu-id="da4b6-266">Rapporter</span><span class="sxs-lookup"><span data-stu-id="da4b6-266">Reports</span></span>

<span data-ttu-id="da4b6-267">När du är klar ändra prioriteringarna och uppdaterar hello status för alla genererade hot, du kan spara hello-filen och/eller skriva ut rapporten genom att gå för ”rapportera” och sedan ”skapa fullständiga rapporten”.</span><span class="sxs-lookup"><span data-stu-id="da4b6-267">Once you're done changing priorities and updating hello status of each generated threat, you can save hello file and/or print out a report by going too"Report" and then "Create Full Report."</span></span> <span data-ttu-id="da4b6-268">Blir du ombedd tooname hello rapporten och när du gör det, bör du se något liknande toohello bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="da4b6-268">You'll be asked tooname hello report, and once you do, you should see something similar toohello image below:</span></span>

![Rapport](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a><span data-ttu-id="da4b6-270">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da4b6-270">Next steps</span></span>

<span data-ttu-id="da4b6-271">toocontribute en mall för hello community gå tooour  **[GitHub](https://github.com/Microsoft/threat-modeling-templates)**  sidan.</span><span class="sxs-lookup"><span data-stu-id="da4b6-271">toocontribute a template for hello community, please go tooour **[GitHub](https://github.com/Microsoft/threat-modeling-templates)** page.</span></span> <span data-ttu-id="da4b6-272">**[Hämta](https://aka.ms/tmtpreview)**  hello verktyget tooget redan idag.</span><span class="sxs-lookup"><span data-stu-id="da4b6-272">**[Download](https://aka.ms/tmtpreview)** hello tool tooget started today.</span></span>
