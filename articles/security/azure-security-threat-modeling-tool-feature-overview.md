---
title: Microsoft Threat Modeling verktyget - Azure | Microsoft Docs
description: "Lär dig mer om alla funktioner som finns i verktyget Modeling hot"
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
ms.openlocfilehash: 621ff305d7e782f85eeaae6c3fb02031673549c6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="threat-modeling-tool-feature-overview"></a><span data-ttu-id="6bd84-103">Översikt över hot Modeling verktyget funktioner</span><span class="sxs-lookup"><span data-stu-id="6bd84-103">Threat Modeling Tool feature overview</span></span>

<span data-ttu-id="6bd84-104">Vi är glada över att du väljer att använda verktyget hot modellering för ditt hot modeling behov!</span><span class="sxs-lookup"><span data-stu-id="6bd84-104">We are glad you chose to use the Threat Modeling Tool for your threat modeling needs!</span></span> <span data-ttu-id="6bd84-105">Om du inte gjort det, gå  **[komma igång med verktyget Modeling hot](./azure-security-threat-modeling-tool-getting-started.md)**  att lära dig grunderna.</span><span class="sxs-lookup"><span data-stu-id="6bd84-105">If you haven’t done so, visit **[Getting Started with the Threat Modeling Tool](./azure-security-threat-modeling-tool-getting-started.md)** to learn the basics.</span></span>

> <span data-ttu-id="6bd84-106">Vårt verktyg uppdateras ofta, så kontrollera den här guiden ofta om du vill se vår senaste funktioner och förbättringar.</span><span class="sxs-lookup"><span data-stu-id="6bd84-106">Our tool is updated often, so check this guide often to see our latest features and improvements.</span></span>

<span data-ttu-id="6bd84-107">Klicka på knappen ”Skapa en ny modell” öppnar en tom startsida som liknar bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="6bd84-107">Clicking on the "Create a New Model" button opens a blank start page, similar to the image below:</span></span>

![Tom startsida](./media/azure-security-threat-modeling-tool/tmtstart.png)

<span data-ttu-id="6bd84-109">Använda modellen hot som skapats av vårt team i den  **[komma igång](./azure-security-threat-modeling-tool-getting-started.md)**  exemplet ska vi ta en titt alla funktioner som finns i verktyget idag.</span><span class="sxs-lookup"><span data-stu-id="6bd84-109">Using the threat model created by our team in the **[Getting Started](./azure-security-threat-modeling-tool-getting-started.md)** example, let’s check out all the features available in the tool today.</span></span>

![Grundläggande Hotmodell](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a><span data-ttu-id="6bd84-111">Navigering</span><span class="sxs-lookup"><span data-stu-id="6bd84-111">Navigation</span></span>

<span data-ttu-id="6bd84-112">Innan du dyker in de inbyggda funktionerna ska vi gå igenom huvudkomponenterna i verktyget</span><span class="sxs-lookup"><span data-stu-id="6bd84-112">Before diving into the built-in features, let’s go over the main components found in the tool</span></span>

### <a name="menu-items"></a><span data-ttu-id="6bd84-113">Menyalternativ</span><span class="sxs-lookup"><span data-stu-id="6bd84-113">Menu items</span></span>

<span data-ttu-id="6bd84-114">Upplevelsen bör likna andra Microsoft-produkter.</span><span class="sxs-lookup"><span data-stu-id="6bd84-114">The experience should be similar to other Microsoft products.</span></span> <span data-ttu-id="6bd84-115">Vi börjar med att gå igenom de översta menyalternativen:</span><span class="sxs-lookup"><span data-stu-id="6bd84-115">Let’s begin by going through the top-level menu items:</span></span>

![Menyalternativ](./media/azure-security-threat-modeling-tool/menuitems.png)

| <span data-ttu-id="6bd84-117">Etikett</span><span class="sxs-lookup"><span data-stu-id="6bd84-117">Label</span></span>                               | <span data-ttu-id="6bd84-118">Information</span><span class="sxs-lookup"><span data-stu-id="6bd84-118">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="6bd84-119">**Fil**</span><span class="sxs-lookup"><span data-stu-id="6bd84-119">**File**</span></span> | <ul><li><span data-ttu-id="6bd84-120">Öppna, spara och stänga filer</span><span class="sxs-lookup"><span data-stu-id="6bd84-120">Open, Save and Close Files</span></span></li><li><span data-ttu-id="6bd84-121">Logga in/ut av OneDrive konton</span><span class="sxs-lookup"><span data-stu-id="6bd84-121">Sign In/Out of OneDrive accounts</span></span></li><li><span data-ttu-id="6bd84-122">Dela länkar (visa + redigera)</span><span class="sxs-lookup"><span data-stu-id="6bd84-122">Share Links (View + Edit)</span></span></li><li><span data-ttu-id="6bd84-123">Visa filinformation</span><span class="sxs-lookup"><span data-stu-id="6bd84-123">View File Information</span></span></li><li><span data-ttu-id="6bd84-124">Lägga till nya mall i befintliga modeller</span><span class="sxs-lookup"><span data-stu-id="6bd84-124">Apply New Template to Existing Models</span></span></li></ul> |
| <span data-ttu-id="6bd84-125">**Redigera**</span><span class="sxs-lookup"><span data-stu-id="6bd84-125">**Edit**</span></span> | <span data-ttu-id="6bd84-126">Ångra/Gör om åtgärder, som även en kopia, klistra in och ta bort</span><span class="sxs-lookup"><span data-stu-id="6bd84-126">Undo/redo actions, as well a copy, paste and delete</span></span> |
| <span data-ttu-id="6bd84-127">**Visa**</span><span class="sxs-lookup"><span data-stu-id="6bd84-127">**View**</span></span> | <ul><li><span data-ttu-id="6bd84-128">Växla mellan **Analysis** och **Design** vyer</span><span class="sxs-lookup"><span data-stu-id="6bd84-128">Switch between **Analysis** and **Design** views</span></span></li><li><span data-ttu-id="6bd84-129">Öppna stängd windows (e.g.stencils, egenskaper och meddelanden)</span><span class="sxs-lookup"><span data-stu-id="6bd84-129">Open closed windows (e.g.stencils, element properties and messages)</span></span></li><li><span data-ttu-id="6bd84-130">Återställ layout till standardinställningarna</span><span class="sxs-lookup"><span data-stu-id="6bd84-130">Reset layout to default settings</span></span></li></ul> |
| <span data-ttu-id="6bd84-131">**Diagram**</span><span class="sxs-lookup"><span data-stu-id="6bd84-131">**Diagram**</span></span> | <span data-ttu-id="6bd84-132">Lägg till/ta bort diagram och navigera genom ”flikar” av diagram</span><span class="sxs-lookup"><span data-stu-id="6bd84-132">Add/Delete diagrams and navigate through “tabs” of diagrams</span></span> |
| <span data-ttu-id="6bd84-133">**Rapporter**</span><span class="sxs-lookup"><span data-stu-id="6bd84-133">**Reports**</span></span> | <span data-ttu-id="6bd84-134">Skapa HTML-rapporter för att dela med andra</span><span class="sxs-lookup"><span data-stu-id="6bd84-134">Create HTML reports to share with others</span></span> |
| <span data-ttu-id="6bd84-135">**Hjälp**</span><span class="sxs-lookup"><span data-stu-id="6bd84-135">**Help**</span></span> | <span data-ttu-id="6bd84-136">Hjälper som hjälper dig att använda verktyget</span><span class="sxs-lookup"><span data-stu-id="6bd84-136">Guides to help you use the tool</span></span> |

<span data-ttu-id="6bd84-137">Ikonerna är genvägar i översta menyer:</span><span class="sxs-lookup"><span data-stu-id="6bd84-137">The icons are shortcuts for the top-level menus:</span></span>

| <span data-ttu-id="6bd84-138">Ikon</span><span class="sxs-lookup"><span data-stu-id="6bd84-138">Icon</span></span>                               | <span data-ttu-id="6bd84-139">Information</span><span class="sxs-lookup"><span data-stu-id="6bd84-139">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="6bd84-140">**Öppna**</span><span class="sxs-lookup"><span data-stu-id="6bd84-140">**Open**</span></span> | <span data-ttu-id="6bd84-141">Öppnar en ny fil</span><span class="sxs-lookup"><span data-stu-id="6bd84-141">Opens a new file</span></span> |
| <span data-ttu-id="6bd84-142">**Spara**</span><span class="sxs-lookup"><span data-stu-id="6bd84-142">**Save**</span></span> | <span data-ttu-id="6bd84-143">Sparar aktuell fil</span><span class="sxs-lookup"><span data-stu-id="6bd84-143">Saves current file</span></span> |
| <span data-ttu-id="6bd84-144">**Design**</span><span class="sxs-lookup"><span data-stu-id="6bd84-144">**Design**</span></span> | <span data-ttu-id="6bd84-145">Försätts i designvyn, där du kan skapa modeller</span><span class="sxs-lookup"><span data-stu-id="6bd84-145">Goes into design view, where you can create models</span></span> |
| <span data-ttu-id="6bd84-146">**Analysera**</span><span class="sxs-lookup"><span data-stu-id="6bd84-146">**Analyze**</span></span> | <span data-ttu-id="6bd84-147">Visar genererade hot och deras egenskaper</span><span class="sxs-lookup"><span data-stu-id="6bd84-147">Shows generated threats and their properties</span></span> |
| <span data-ttu-id="6bd84-148">**Lägg till Diagram**</span><span class="sxs-lookup"><span data-stu-id="6bd84-148">**Add Diagram**</span></span> | <span data-ttu-id="6bd84-149">Lägger till nya diagram (liknar nya flikar i Excel)</span><span class="sxs-lookup"><span data-stu-id="6bd84-149">Adds new diagram (similar to new tabs in Excel)</span></span> |
| <span data-ttu-id="6bd84-150">**Ta bort Diagram**</span><span class="sxs-lookup"><span data-stu-id="6bd84-150">**Delete Diagram**</span></span> | <span data-ttu-id="6bd84-151">Tar bort aktuella diagrammet</span><span class="sxs-lookup"><span data-stu-id="6bd84-151">Deletes current diagram</span></span> |
| <span data-ttu-id="6bd84-152">**Kopiera och klipp ut/klistra in**</span><span class="sxs-lookup"><span data-stu-id="6bd84-152">**Copy/Cut/Paste**</span></span> | <span data-ttu-id="6bd84-153">Klistrar in-kopior/delar element</span><span class="sxs-lookup"><span data-stu-id="6bd84-153">Copies/cuts/pastes elements</span></span> |
| <span data-ttu-id="6bd84-154">**Ångra/Gör om**</span><span class="sxs-lookup"><span data-stu-id="6bd84-154">**Undo/Redo**</span></span> | <span data-ttu-id="6bd84-155">Ångrar/gör om åtgärder</span><span class="sxs-lookup"><span data-stu-id="6bd84-155">Undoes/redoes actions</span></span> |
| <span data-ttu-id="6bd84-156">**Zooma In / Zooma ut**</span><span class="sxs-lookup"><span data-stu-id="6bd84-156">**Zoom In/Zoom Out**</span></span> | <span data-ttu-id="6bd84-157">Zoomar in och ut diagram för en bättre vy</span><span class="sxs-lookup"><span data-stu-id="6bd84-157">Zooms in and out of the diagram for a better view</span></span> |
| <span data-ttu-id="6bd84-158">**Feedback**</span><span class="sxs-lookup"><span data-stu-id="6bd84-158">**Feedback**</span></span> | <span data-ttu-id="6bd84-159">Öppnar MSDN-Forum</span><span class="sxs-lookup"><span data-stu-id="6bd84-159">Opens the MSDN Forum</span></span> |

### <a name="canvas"></a><span data-ttu-id="6bd84-160">Arbetsytan</span><span class="sxs-lookup"><span data-stu-id="6bd84-160">Canvas</span></span>

<span data-ttu-id="6bd84-161">Utrymme som du drar och släpper element i.</span><span class="sxs-lookup"><span data-stu-id="6bd84-161">The space where you drag and drop elements into.</span></span> <span data-ttu-id="6bd84-162">Dra och släpp är det snabbaste och mest effektiva sättet att bygga modeller.</span><span class="sxs-lookup"><span data-stu-id="6bd84-162">Drag and drop is the quickest and most efficient way to build models.</span></span> <span data-ttu-id="6bd84-163">Du kan också högerklicka och välj på menyn som lägger till allmän versioner av element som du använder, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="6bd84-163">You may also right click and select from the menu, which adds generic versions of the elements you’re using, as shown below.</span></span>

#### <a name="dropping-the-stencil-on-the-canvas"></a><span data-ttu-id="6bd84-164">Släppa stencilen på arbetsytan</span><span class="sxs-lookup"><span data-stu-id="6bd84-164">Dropping the stencil on the canvas</span></span>

![Arbetsytan släpp](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-the-stencil"></a><span data-ttu-id="6bd84-166">Klicka på stencilen</span><span class="sxs-lookup"><span data-stu-id="6bd84-166">Clicking on the stencil</span></span>

![Egenskaper](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a><span data-ttu-id="6bd84-168">Stenciler</span><span class="sxs-lookup"><span data-stu-id="6bd84-168">Stencils</span></span>

<span data-ttu-id="6bd84-169">Var du hittar alla stenciler som kan användas baserat på den valda mallen.</span><span class="sxs-lookup"><span data-stu-id="6bd84-169">Where you can find all stencils available to use based on the template selected.</span></span> <span data-ttu-id="6bd84-170">Om du inte hittar rätt element, försök med en annan mall eller ändra en så att de passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="6bd84-170">If you can’t find the right elements, try using another template, or modify one to fit your needs.</span></span> <span data-ttu-id="6bd84-171">I allmänhet kan ska du kunna hitta en kombination av kategorier med de nedan:</span><span class="sxs-lookup"><span data-stu-id="6bd84-171">Generally, you should be able to find a combination of categories like the ones below:</span></span>

| <span data-ttu-id="6bd84-172">Stencilens namn</span><span class="sxs-lookup"><span data-stu-id="6bd84-172">Stencil Name</span></span>                               | <span data-ttu-id="6bd84-173">Information</span><span class="sxs-lookup"><span data-stu-id="6bd84-173">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="6bd84-174">**Processen**</span><span class="sxs-lookup"><span data-stu-id="6bd84-174">**Process**</span></span> | <span data-ttu-id="6bd84-175">Program, webbläsare plugin-program, trådar, virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="6bd84-175">Applications, Browser Plugins, Threads, Virtual Machines</span></span> |
| <span data-ttu-id="6bd84-176">**Extern kontakt**</span><span class="sxs-lookup"><span data-stu-id="6bd84-176">**External Interactor**</span></span> | <span data-ttu-id="6bd84-177">Autentiseringsproviders, webbläsare, användare, webbprogram</span><span class="sxs-lookup"><span data-stu-id="6bd84-177">Authentication Providers, Browsers, Users, Web Applications</span></span> |
| <span data-ttu-id="6bd84-178">**Datalager**</span><span class="sxs-lookup"><span data-stu-id="6bd84-178">**Data Store**</span></span> | <span data-ttu-id="6bd84-179">Cache, lagring, Configuration-filer, databaser, registret</span><span class="sxs-lookup"><span data-stu-id="6bd84-179">Cache, Storage, Configuration Files, Databases, Registry</span></span> |
| <span data-ttu-id="6bd84-180">**Dataflöde**</span><span class="sxs-lookup"><span data-stu-id="6bd84-180">**Data Flow**</span></span> | <span data-ttu-id="6bd84-181">Binär, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, namngivna Pipe, RPC/DCOM, SMB, UDP</span><span class="sxs-lookup"><span data-stu-id="6bd84-181">Binary, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP</span></span> |
| <span data-ttu-id="6bd84-182">**Förtroende kantlinje/gräns**</span><span class="sxs-lookup"><span data-stu-id="6bd84-182">**Trust Line/Border Boundary**</span></span> | <span data-ttu-id="6bd84-183">Företagets nätverk, Internet, dator, Sandbox, användar-/ Kernel-läge</span><span class="sxs-lookup"><span data-stu-id="6bd84-183">Corporate Networks, Internet, Machine, Sandbox, User/Kernel Mode</span></span> |

### <a name="notesmessages"></a><span data-ttu-id="6bd84-184">Anteckningar/meddelanden</span><span class="sxs-lookup"><span data-stu-id="6bd84-184">Notes/Messages</span></span>

| <span data-ttu-id="6bd84-185">Komponent</span><span class="sxs-lookup"><span data-stu-id="6bd84-185">Component</span></span>                               | <span data-ttu-id="6bd84-186">Information</span><span class="sxs-lookup"><span data-stu-id="6bd84-186">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="6bd84-187">**Meddelanden**</span><span class="sxs-lookup"><span data-stu-id="6bd84-187">**Messages**</span></span> | <span data-ttu-id="6bd84-188">Interna verktyget logik som varnar användare när det uppstår ett fel, till exempel inga data som flödar mellan element</span><span class="sxs-lookup"><span data-stu-id="6bd84-188">Internal tool logic that alerts users whenever there is an error, such as no data flows between elements</span></span> |
| <span data-ttu-id="6bd84-189">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="6bd84-189">**Notes**</span></span> | <span data-ttu-id="6bd84-190">Manuell anteckningar som lagts till i filen med teknikerna snappa under hela processen design och granska</span><span class="sxs-lookup"><span data-stu-id="6bd84-190">Manual notes added to the file by engineering teams throughout the design and review process</span></span> |

### <a name="element-properties"></a><span data-ttu-id="6bd84-191">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="6bd84-191">Element properties</span></span>

<span data-ttu-id="6bd84-192">Dessa varierar beroende på markerade element.</span><span class="sxs-lookup"><span data-stu-id="6bd84-192">These vary by the elements selected.</span></span> <span data-ttu-id="6bd84-193">Förutom förtroende gränser kan innehålla alla element 3 allmänna inställningar:</span><span class="sxs-lookup"><span data-stu-id="6bd84-193">Apart from Trust Boundaries, all other elements contain 3 general selections:</span></span>

| <span data-ttu-id="6bd84-194">Egenskapen element</span><span class="sxs-lookup"><span data-stu-id="6bd84-194">Element Property</span></span>                               | <span data-ttu-id="6bd84-195">Information</span><span class="sxs-lookup"><span data-stu-id="6bd84-195">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="6bd84-196">**Namn**</span><span class="sxs-lookup"><span data-stu-id="6bd84-196">**Name**</span></span> | <span data-ttu-id="6bd84-197">Användbar för att namnge din processer, Arkiv, interaktörer och flöden för att enkelt identifiera</span><span class="sxs-lookup"><span data-stu-id="6bd84-197">Useful for naming your processes, stores, interactors and flows to be easily recognized</span></span> |
| <span data-ttu-id="6bd84-198">**Utanför omfånget**</span><span class="sxs-lookup"><span data-stu-id="6bd84-198">**Out of Scope**</span></span> | <span data-ttu-id="6bd84-199">Om markerad hämtas elementet utanför matrisen hot generation (rekommenderas inte)</span><span class="sxs-lookup"><span data-stu-id="6bd84-199">If selected, the element is taken out of the threat generation matrix (not recommended)</span></span> |
| <span data-ttu-id="6bd84-200">**Orsak till utanför omfånget**</span><span class="sxs-lookup"><span data-stu-id="6bd84-200">**Reason for Out of Scope**</span></span> | <span data-ttu-id="6bd84-201">Motiveringen fält så att användarna vet varför täcker har valts</span><span class="sxs-lookup"><span data-stu-id="6bd84-201">Justification field to let users know why out of scope was selected</span></span> |

<span data-ttu-id="6bd84-202">Egenskaper har ändrats under varje element.</span><span class="sxs-lookup"><span data-stu-id="6bd84-202">Properties are changed under each element category.</span></span> <span data-ttu-id="6bd84-203">Klicka på varje element att granska de tillgängliga alternativen eller öppna mallen om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="6bd84-203">Click on each element to inspect the available options, or open the template to learn more.</span></span> <span data-ttu-id="6bd84-204">Det är dags till funktionerna.</span><span class="sxs-lookup"><span data-stu-id="6bd84-204">Let’s get into the features.</span></span>

## <a name="welcome-screen"></a><span data-ttu-id="6bd84-205">Välkomstskärmen</span><span class="sxs-lookup"><span data-stu-id="6bd84-205">Welcome screen</span></span>

<span data-ttu-id="6bd84-206">Välkomstskärmen är det första som visas när du öppnar appen.</span><span class="sxs-lookup"><span data-stu-id="6bd84-206">The welcome screen is the first thing you see when you open the app.</span></span>

### <a name="open-a-model"></a><span data-ttu-id="6bd84-207">Öppna en modell</span><span class="sxs-lookup"><span data-stu-id="6bd84-207">Open A model</span></span>

<span data-ttu-id="6bd84-208">Hovra över knappen ”Öppna en modell” visar 2 dolda alternativ: ”Öppna från den här datorn” och ”öppna från OneDrive”.</span><span class="sxs-lookup"><span data-stu-id="6bd84-208">Hovering over “Open a Model” button shows you 2 hidden options: “Open From this Computer” and “Open from OneDrive.”</span></span> <span data-ttu-id="6bd84-209">Först öppnar öppna skärmen, medan andra tar dig igenom processen inloggningen för OneDrive, så att du kan välja mappar och filer efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="6bd84-209">The first opens the File Open screen, while the second takes you through the sign in process for OneDrive, allowing you to pick folders and files after a successful authentication.</span></span>

![Öppna modellen](./media/azure-security-threat-modeling-tool/openmodel.png)

![Öppna från datorn eller OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a><span data-ttu-id="6bd84-212">Feedback, förslag och problem</span><span class="sxs-lookup"><span data-stu-id="6bd84-212">Feedback, suggestions and issues</span></span>

<span data-ttu-id="6bd84-213">Det här alternativet tar dig vidare till MSDN-forum för SDL-verktyg.</span><span class="sxs-lookup"><span data-stu-id="6bd84-213">Selecting this option will take you to the MSDN Forums for SDL Tools.</span></span> <span data-ttu-id="6bd84-214">Det är ett bra sätt att Kolla in vad andra säger om verktyget, inklusive lösningar och nya idéer.</span><span class="sxs-lookup"><span data-stu-id="6bd84-214">It’s a great way to check out what other people are saying about the tool, including workarounds and new ideas.</span></span>

![Feedback](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a><span data-ttu-id="6bd84-216">Designvyn</span><span class="sxs-lookup"><span data-stu-id="6bd84-216">Design view</span></span>

<span data-ttu-id="6bd84-217">När du öppnar eller skapa en ny modell, vidarebefordras du till designvyn.</span><span class="sxs-lookup"><span data-stu-id="6bd84-217">Whenever you open or create a new model, you’ll be taken to the design view.</span></span>

### <a name="adding-elements"></a><span data-ttu-id="6bd84-218">Att lägga till element</span><span class="sxs-lookup"><span data-stu-id="6bd84-218">Adding elements</span></span>

<span data-ttu-id="6bd84-219">Det finns 2 sätt att lägga till element i rutnätet:</span><span class="sxs-lookup"><span data-stu-id="6bd84-219">There are 2 ways to add elements on the grid:</span></span>

- <span data-ttu-id="6bd84-220">**Dra och släpp** – dra det önskade elementet i rutnätet och sedan använda elementegenskaper för att ange ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="6bd84-220">**Drag and Drop** – drag the desired element to the grid, then use the element properties to provide additional information.</span></span>
- <span data-ttu-id="6bd84-221">**Högerklicka på** – högerklickar du någonstans på rutnätet och välj den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="6bd84-221">**Right Click** – right click anywhere on the grid and select from the dropdown menu.</span></span> <span data-ttu-id="6bd84-222">En allmän representation av elementet visas på skärmen.</span><span class="sxs-lookup"><span data-stu-id="6bd84-222">A generic representation of that element will appear on the screen.</span></span>

### <a name="connecting-elements"></a><span data-ttu-id="6bd84-223">Ansluta element</span><span class="sxs-lookup"><span data-stu-id="6bd84-223">Connecting elements</span></span>

<span data-ttu-id="6bd84-224">Det finns 2 sätt att ansluta element i verktyget:</span><span class="sxs-lookup"><span data-stu-id="6bd84-224">There are 2 ways to connect elements in the tool:</span></span>

- <span data-ttu-id="6bd84-225">**Dra och släpp** – dra önskade dataflöde i rutnätet och ansluta båda ändarna till lämplig elementen.</span><span class="sxs-lookup"><span data-stu-id="6bd84-225">**Drag and Drop** – drag the desired dataflow to the grid, and connect both ends to the appropriate elements.</span></span>
- <span data-ttu-id="6bd84-226">**Klicka på + SKIFT** – Klicka på det första elementet (skicka data), tryck och håll ned SKIFT och markerar sedan det andra elementet (mottagning av data).</span><span class="sxs-lookup"><span data-stu-id="6bd84-226">**Click + Shift** – click on the first element (sending data), press and hold the Shift key, then select the second element (receiving data).</span></span> <span data-ttu-id="6bd84-227">Högerklicka och Välj ”Anslut”.</span><span class="sxs-lookup"><span data-stu-id="6bd84-227">Right click, and select “Connect.”</span></span> <span data-ttu-id="6bd84-228">Ordningen är inte lika viktigt om du använder en dubbelriktad dataflöde.</span><span class="sxs-lookup"><span data-stu-id="6bd84-228">If you’re using a bi-directional dataflow, the order is not as important.</span></span>

### <a name="properties"></a><span data-ttu-id="6bd84-229">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="6bd84-229">Properties</span></span>

<span data-ttu-id="6bd84-230">Visar alla egenskaper som kan ändras på stencilerna placeras i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="6bd84-230">Shows all the properties that can be modified on the stencils placed in the diagram.</span></span> <span data-ttu-id="6bd84-231">Om du vill visa egenskaper, klicka på stencilen och informationen fylls i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="6bd84-231">To see the properties, just click on the stencil and the information will be populated accordingly.</span></span> <span data-ttu-id="6bd84-232">Exemplet nedan visar före och efter ett ”databas” stencil dras till diagrammet:</span><span class="sxs-lookup"><span data-stu-id="6bd84-232">The example below shows before and after a "Database" stencil is dragged onto the diagram:</span></span>

#### <a name="before"></a><span data-ttu-id="6bd84-233">Innan du</span><span class="sxs-lookup"><span data-stu-id="6bd84-233">Before</span></span>

![Innan du](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a><span data-ttu-id="6bd84-235">Efter</span><span class="sxs-lookup"><span data-stu-id="6bd84-235">After</span></span>

![Efter](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a><span data-ttu-id="6bd84-237">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="6bd84-237">Messages</span></span>

<span data-ttu-id="6bd84-238">Om du skapar en hotmodell och glömt att ansluta dataflöden till element meddelar dig att agera i meddelandefönstret. Du kan välja att ignorera det och följ instruktionerna för att åtgärda problemet.</span><span class="sxs-lookup"><span data-stu-id="6bd84-238">If you create a threat model and forget to connect data flows to elements, the message window notifies you to act. You can choose to ignore it or follow the instructions to fix the issue.</span></span> 

![Meddelanden](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a><span data-ttu-id="6bd84-240">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="6bd84-240">Notes</span></span>

<span data-ttu-id="6bd84-241">Växla flikar från meddelanden till anteckningar kan du lägga till anteckningar i diagrammet för att avbilda dina tankar</span><span class="sxs-lookup"><span data-stu-id="6bd84-241">Switching tabs from Messages to Notes allows you to add notes to your diagram to capture all your thoughts</span></span>

## <a name="analysis-view"></a><span data-ttu-id="6bd84-242">Analysen</span><span class="sxs-lookup"><span data-stu-id="6bd84-242">Analysis view</span></span>

<span data-ttu-id="6bd84-243">När du är klar skapar diagrammet växla till analys genom att gå till de översta menyn Inställningar och välja förstoringsglaset bredvid paletten paint.</span><span class="sxs-lookup"><span data-stu-id="6bd84-243">Once you're done building your diagram, switch over to analysis view by going to the top menu selections and choosing the magnifying glass next to the paint palette.</span></span>

![Analysen](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a><span data-ttu-id="6bd84-245">Val av genererade hot</span><span class="sxs-lookup"><span data-stu-id="6bd84-245">Generated threat selection</span></span>

<span data-ttu-id="6bd84-246">När du klickar på ett hot kan du utnyttja tre unika funktioner:</span><span class="sxs-lookup"><span data-stu-id="6bd84-246">When you click on a threat, you can leverage three unique functions:</span></span>

| <span data-ttu-id="6bd84-247">Funktion</span><span class="sxs-lookup"><span data-stu-id="6bd84-247">Feature</span></span>                               | <span data-ttu-id="6bd84-248">Information</span><span class="sxs-lookup"><span data-stu-id="6bd84-248">Info</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="6bd84-249">**Läs indikator**</span><span class="sxs-lookup"><span data-stu-id="6bd84-249">**Read Indicator**</span></span> | <p><span data-ttu-id="6bd84-250">Hot har nu markerats som läst som enkelt kan hjälpa dig att hålla reda på de objekt som du redan har gått igenom</span><span class="sxs-lookup"><span data-stu-id="6bd84-250">Threat is now marked as read, which can easily help you keep track of the items you already went through</span></span></p><p>![Läs oläst indikator](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| <span data-ttu-id="6bd84-252">**Interaktion fokus**</span><span class="sxs-lookup"><span data-stu-id="6bd84-252">**Interaction Focus**</span></span> | <p><span data-ttu-id="6bd84-253">Interaktion i diagrammet som hör till det hotet är markerat</span><span class="sxs-lookup"><span data-stu-id="6bd84-253">Interaction in the diagram belonging to that threat is highlighted</span></span></p><p>![Interaktion fokus](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| <span data-ttu-id="6bd84-255">**Egenskaper för hot**</span><span class="sxs-lookup"><span data-stu-id="6bd84-255">**Threat Properties**</span></span> | <p><span data-ttu-id="6bd84-256">Mer information om hot fylls i egenskapsfönstret för hot</span><span class="sxs-lookup"><span data-stu-id="6bd84-256">Additional information about the threat is populated in the threat properties window</span></span></p><p>![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a><span data-ttu-id="6bd84-258">Ändra prioritet</span><span class="sxs-lookup"><span data-stu-id="6bd84-258">Priority change</span></span>

<span data-ttu-id="6bd84-259">Ändrar prioritetsnivån för varje genererade hot ändras deras färger så att den blir lättare att identifiera hot som hög, medel och låg prioritet.</span><span class="sxs-lookup"><span data-stu-id="6bd84-259">Changing the priority level of each generated threat also changes their colors to make it easy to identify high, medium and low priority threats.</span></span>

![Ändra prioritet](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a><span data-ttu-id="6bd84-261">Redigerbart fält för hot egenskaper</span><span class="sxs-lookup"><span data-stu-id="6bd84-261">Threat properties editable fields</span></span>

<span data-ttu-id="6bd84-262">Som det visas i bilden ovan användare kan ändra den information som genereras av verktyget en också lägga till information i vissa fält, till exempel justering.</span><span class="sxs-lookup"><span data-stu-id="6bd84-262">As seen in the image above, users can change the information generated by the tool an also add information to certain fields, such as justification.</span></span> <span data-ttu-id="6bd84-263">De här fälten genereras av mallen, så att om du behöver mer information för varje hot kan du uppmanas att göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="6bd84-263">These fields are generated by the template, so if you need more information for each threat, you're encouraged to make modifications.</span></span>

![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a><span data-ttu-id="6bd84-265">Rapporter</span><span class="sxs-lookup"><span data-stu-id="6bd84-265">Reports</span></span>

<span data-ttu-id="6bd84-266">När du är klar ändra prioriteringarna och uppdaterar statusen för varje genererade hot, du kan spara filen och/eller skriva ut rapporten genom att gå till ”rapport” och sedan ”skapa fullständiga”.</span><span class="sxs-lookup"><span data-stu-id="6bd84-266">Once you're done changing priorities and updating the status of each generated threat, you can save the file and/or print out a report by going to "Report" and then "Create Full Report."</span></span> <span data-ttu-id="6bd84-267">Blir du ombedd att namnge rapporten och när du gör det, bör du se något som liknar bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="6bd84-267">You'll be asked to name the report, and once you do, you should see something similar to the image below:</span></span>

![Rapport](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a><span data-ttu-id="6bd84-269">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6bd84-269">Next steps</span></span>

<span data-ttu-id="6bd84-270">Om du vill bidra med en mall för gemenskapen, gå till vår  **[GitHub](https://github.com/Microsoft/threat-modeling-templates)**  sidan.</span><span class="sxs-lookup"><span data-stu-id="6bd84-270">To contribute a template for the community, please go to our **[GitHub](https://github.com/Microsoft/threat-modeling-templates)** page.</span></span> <span data-ttu-id="6bd84-271">**[Hämta](https://aka.ms/tmtpreview)**  verktyget för att starta redan idag.</span><span class="sxs-lookup"><span data-stu-id="6bd84-271">**[Download](https://aka.ms/tmtpreview)** the tool to get started today.</span></span>
