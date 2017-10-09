---
title: aaaAzure funktioner Runtime-Installation | Microsoft Docs
description: Hur tooInstall hello Azure Functions-Runtime
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 67c6d10b5c0ac43e880d29cff0ae7b099f82bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-functions-runtime-preview"></a><span data-ttu-id="5c744-103">Installera hello Azure Functions Runtime Preview</span><span class="sxs-lookup"><span data-stu-id="5c744-103">Install hello Azure Functions Runtime Preview</span></span>

<span data-ttu-id="5c744-104">Om du vill tooinstall hello Azure Functions-Runtime preview måste du följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="5c744-104">If you would like tooinstall hello Azure Functions Runtime preview, you must follow these steps:</span></span>

1. <span data-ttu-id="5c744-105">Se till att din dator skickar hello minimikrav</span><span class="sxs-lookup"><span data-stu-id="5c744-105">Ensure your machine passes hello minimum requirements</span></span>
1. <span data-ttu-id="5c744-106">Hämta hello [Azure Functions Runtime Preview Installer](https://aka.ms/azafr).</span><span class="sxs-lookup"><span data-stu-id="5c744-106">Download hello [Azure Functions Runtime Preview Installer](https://aka.ms/azafr).</span></span> 
1. <span data-ttu-id="5c744-107">Installera hello Azure Functions-Runtime preview</span><span class="sxs-lookup"><span data-stu-id="5c744-107">Install hello Azure Functions Runtime preview</span></span>
1. <span data-ttu-id="5c744-108">Fullständig hello konfigurationen av hello Azure Functions-Runtime preview</span><span class="sxs-lookup"><span data-stu-id="5c744-108">Complete hello configuration of hello Azure Functions Runtime preview</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c744-109">Krav</span><span class="sxs-lookup"><span data-stu-id="5c744-109">Prerequisites</span></span>

<span data-ttu-id="5c744-110">Innan du installerar hello Azure Functions-Runtime preview måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="5c744-110">Before you install hello Azure Functions Runtime preview, you must have hello following:</span></span>

1. <span data-ttu-id="5c744-111">En dator som kör Microsoft Windows Server 2016 eller Microsoft Windows 10 skapare Update (Professional och Enterprise Edition).</span><span class="sxs-lookup"><span data-stu-id="5c744-111">A machine running Microsoft Windows Server 2016 or Microsoft Windows 10 Creators Update (Professional or Enterprise Edition).</span></span>
1. <span data-ttu-id="5c744-112">En SQL Server-instans som körs i nätverket.</span><span class="sxs-lookup"><span data-stu-id="5c744-112">A SQL Server instance running within your network.</span></span>  <span data-ttu-id="5c744-113">Krav på lägsta version är SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="5c744-113">Minimum edition requirement is SQL Server Express.</span></span>

## <a name="install-hello-azure-functions-runtime-preview"></a><span data-ttu-id="5c744-114">Installera hello Azure Functions Runtime Preview</span><span class="sxs-lookup"><span data-stu-id="5c744-114">Install hello Azure Functions Runtime Preview</span></span>

<span data-ttu-id="5c744-115">hello Azure Functions-Runtime preview installer hjälper dig att hello installation av hello Azure Functions-Runtime preview Management- och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="5c744-115">hello Azure Functions Runtime preview installer guides you through hello installation of hello Azure Functions Runtime preview Management and Worker Roles.</span></span>  <span data-ttu-id="5c744-116">Det är möjligt tooinstall hello hantering och Worker-rollen på hello samma dator.</span><span class="sxs-lookup"><span data-stu-id="5c744-116">It is possible tooinstall hello Management and Worker role on hello same machine.</span></span>  <span data-ttu-id="5c744-117">Men när du lägger till fler funktioner, måste du distribuera flera arbetsroller på ytterligare datorer toobe kan tooscale dina funktioner på flera arbetare.</span><span class="sxs-lookup"><span data-stu-id="5c744-117">However, as you add more Functions, you must deploy more worker roles on additional machines toobe able tooscale your functions onto multiple workers.</span></span>

## <a name="install-hello-management-and-worker-role-on-hello-same-machine"></a><span data-ttu-id="5c744-118">Installera hello hantering och Worker-rollen på hello samma dator</span><span class="sxs-lookup"><span data-stu-id="5c744-118">Install hello Management and Worker Role on hello same machine</span></span>

1. <span data-ttu-id="5c744-119">Kör hello Azure Functions Runtime Preview Installer.</span><span class="sxs-lookup"><span data-stu-id="5c744-119">Run hello Azure Functions Runtime Preview Installer.</span></span>

    ![Azure Functions-Runtime Preview Installer][1]

1. <span data-ttu-id="5c744-121">**Klicka på nästa** förskott tidigare hello första skedet av hello installer</span><span class="sxs-lookup"><span data-stu-id="5c744-121">**Click Next** advance past hello first stage of hello installer</span></span>
1. <span data-ttu-id="5c744-122">När du har läst hello användningsvillkor hello **EULA**, **kryssrutan hello** tooaccept hello villkoren och **Klicka på nästa** tooadvance.</span><span class="sxs-lookup"><span data-stu-id="5c744-122">Once you have read hello terms of hello **EULA**, **check hello box** tooaccept hello terms and **click Next** tooadvance.</span></span>
1. <span data-ttu-id="5c744-123">Nu väljer hello roller önskade tooinstall på den här datorn **funktioner Ledningsroll** och/eller **funktioner Arbetsrollen** och **Klicka på nästa**</span><span class="sxs-lookup"><span data-stu-id="5c744-123">Now select hello roles you want tooinstall on this machine **Functions Management Role** and/or **Functions Worker Role** and **Click Next**</span></span>

    ![Azure Functions-Runtime Preview-installationsprogram - urval för Systemroll][3]

    > [!NOTE]
    > <span data-ttu-id="5c744-125">Du kan installera hello **funktioner Arbetsrollen** så, följ instruktionerna på många datorer toodo och välj endast **funktioner Arbetsrollen** i hello installer.</span><span class="sxs-lookup"><span data-stu-id="5c744-125">You can install hello **Functions Worker Role** on many other machines toodo so, follow these instructions, and only select **Functions Worker Role** in hello installer.</span></span>

1. <span data-ttu-id="5c744-126">**Klicka på nästa** toohave hello **Azure Functions Runtime Installer** installera på datorn.</span><span class="sxs-lookup"><span data-stu-id="5c744-126">**Click Next** toohave hello **Azure Functions Runtime Installer** install on your machine.</span></span>
1. <span data-ttu-id="5c744-127">När fullständig hello installationsprogrammet startar hello **konfigurationsverktyget för Azure Functions-Runtime**.</span><span class="sxs-lookup"><span data-stu-id="5c744-127">Once complete hello installer will launch hello **Azure Functions Runtime Configuration tool**.</span></span>

    ![Azure Functions Runtime Preview Installer klar][5]

    > [!NOTE]
    > <span data-ttu-id="5c744-129">Om du installerar på **Windows 10** och hello **behållare** funktionen har inte tidigare har aktiverats, hello **Azure Functions-Runtime** uppmanas av installationsprogrammet tooreboot din dator toocomplete hello installera.</span><span class="sxs-lookup"><span data-stu-id="5c744-129">If you are installing on **Windows 10** and hello **Container** feature has not been previously enabled, hello **Azure Functions Runtime** Installer prompts you tooreboot your machine toocomplete hello install.</span></span>

## <a name="configure-hello-azure-functions-runtime"></a><span data-ttu-id="5c744-130">Konfigurera hello Azure Functions-Runtime</span><span class="sxs-lookup"><span data-stu-id="5c744-130">Configure hello Azure Functions Runtime</span></span>

<span data-ttu-id="5c744-131">toocomplete hello Azure Functions-Runtime-installationen måste du slutföra hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5c744-131">toocomplete hello Azure Functions Runtime installation you must complete hello configuration.</span></span>

1. <span data-ttu-id="5c744-132">Hej **konfigurationsverktyget för Azure Functions-Runtime** visas vilka roller som är installerade på datorn.</span><span class="sxs-lookup"><span data-stu-id="5c744-132">hello **Azure Functions Runtime Configuration Tool** shows which roles are installed on your machine.</span></span>

    ![Azure Functions-Runtime Preview-konfigurationsverktyget][6]

1. <span data-ttu-id="5c744-134">Klicka på hello **databasen** ange hello **anslutningsinformationen för SQL Server-instans** och **på Verkställ**.</span><span class="sxs-lookup"><span data-stu-id="5c744-134">Click hello **Database** tab, enter hello **connection details for your SQL Server Instance** and **click Apply**.</span></span>  <span data-ttu-id="5c744-135">Detta är nödvändigt i ordning toohello Azure Functions-Runtime toocreate en databas toosupport hello Runtime.</span><span class="sxs-lookup"><span data-stu-id="5c744-135">This is required in order toohello Azure Functions Runtime toocreate a database toosupport hello Runtime.</span></span>
    
    ![Azure Functions Runtime Preview databaskonfigurationen][7]

1. <span data-ttu-id="5c744-137">Klicka på hello **autentiseringsuppgifter** fliken.  På den här sidan måste du skapa två nya autentiseringsuppgifter för användning med en filresurs som värd för dina Azure-funktioner.</span><span class="sxs-lookup"><span data-stu-id="5c744-137">Click hello **Credentials** tab.  On this screen you must create two new credentials for use with a FileShare for hosting all your Azure Functions.</span></span>  <span data-ttu-id="5c744-138">**Ange användarnamn och lösenord** kombinationer för hello **resursen filägaren** och för hello **filen resursen användaren** och klicka på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="5c744-138">**Specify Username and Password** combinations for hello **File Share Owner** and for hello **File Share User** and click **Apply**.</span></span>

    ![Azure Functions-Runtime Preview autentiseringsuppgifter][8]

1. <span data-ttu-id="5c744-140">Klicka på hello **filresurs** fliken.  I den här skärmen måste du ange hello information om hello **filresurs plats**.</span><span class="sxs-lookup"><span data-stu-id="5c744-140">Click hello **File Share** tab.  In this screen you must specify hello details of hello **File Share location**.</span></span>  <span data-ttu-id="5c744-141">Detta kan skapas för dig eller Använd en befintlig resurs och på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="5c744-141">This can be created for you or you can use an existing File Share and click **Apply**.</span></span>  <span data-ttu-id="5c744-142">Om du väljer en ny filresurs-plats måste du ange en katalog för användning av hello Azure Functions-Runtime.</span><span class="sxs-lookup"><span data-stu-id="5c744-142">If you select a new File Share location, you must specify a directory for use by hello Azure Functions Runtime.</span></span>
    
    ![Azure Functions Runtime Preview filresurs][9]

1. <span data-ttu-id="5c744-144">Klicka på hello **IIS** fliken.  Den här fliken visar hello detaljerad information om hello webbplatser i IIS som hello Azure Functions Runtime installationen skapas.</span><span class="sxs-lookup"><span data-stu-id="5c744-144">Click hello **IIS** tab.  This tab shows hello details of hello websites in IIS that hello Azure Functions Runtime Installation will create.</span></span>  <span data-ttu-id="5c744-145">**Klicka på Verkställ** toocomplete.</span><span class="sxs-lookup"><span data-stu-id="5c744-145">**Click Apply** toocomplete.</span></span>

    ![Förhandsgranska Azure Functions-Runtime IIS][10]

1. <span data-ttu-id="5c744-147">Klicka på hello **Services** fliken.  Den här fliken visar hello status hello tjänster i Azure Functions-Runtime-installationen.</span><span class="sxs-lookup"><span data-stu-id="5c744-147">Click hello **Services** tab.  This tab shows hello status of hello services in your Azure Functions Runtime installation.</span></span>  <span data-ttu-id="5c744-148">Om efter den inledande konfigurationen hello **Azure Functions värdtjänsten aktivering** körs inte klicka **starta tjänsten**</span><span class="sxs-lookup"><span data-stu-id="5c744-148">If after initial configuration hello **Azure Functions Host Activation Service** is not running click **Start Service**</span></span>

    ![Azure Functions Runtime Preview Configruation klar][11]

1. <span data-ttu-id="5c744-150">Slutligen Bläddra toohello **Azure Functions-Runtime portalen** som`https://<machinename>/`</span><span class="sxs-lookup"><span data-stu-id="5c744-150">Finally browse toohello **Azure Functions Runtime Portal** as `https://<machinename>/`</span></span>

    ![Azure Functions-Runtime Preview-portalen][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png