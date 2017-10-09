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
# <a name="install-hello-azure-functions-runtime-preview"></a>Installera hello Azure Functions Runtime Preview

Om du vill tooinstall hello Azure Functions-Runtime preview måste du följa dessa steg:

1. Se till att din dator skickar hello minimikrav
1. Hämta hello [Azure Functions Runtime Preview Installer](https://aka.ms/azafr). 
1. Installera hello Azure Functions-Runtime preview
1. Fullständig hello konfigurationen av hello Azure Functions-Runtime preview

## <a name="prerequisites"></a>Krav

Innan du installerar hello Azure Functions-Runtime preview måste du ha hello följande:

1. En dator som kör Microsoft Windows Server 2016 eller Microsoft Windows 10 skapare Update (Professional och Enterprise Edition).
1. En SQL Server-instans som körs i nätverket.  Krav på lägsta version är SQL Server Express.

## <a name="install-hello-azure-functions-runtime-preview"></a>Installera hello Azure Functions Runtime Preview

hello Azure Functions-Runtime preview installer hjälper dig att hello installation av hello Azure Functions-Runtime preview Management- och arbetsroller.  Det är möjligt tooinstall hello hantering och Worker-rollen på hello samma dator.  Men när du lägger till fler funktioner, måste du distribuera flera arbetsroller på ytterligare datorer toobe kan tooscale dina funktioner på flera arbetare.

## <a name="install-hello-management-and-worker-role-on-hello-same-machine"></a>Installera hello hantering och Worker-rollen på hello samma dator

1. Kör hello Azure Functions Runtime Preview Installer.

    ![Azure Functions-Runtime Preview Installer][1]

1. **Klicka på nästa** förskott tidigare hello första skedet av hello installer
1. När du har läst hello användningsvillkor hello **EULA**, **kryssrutan hello** tooaccept hello villkoren och **Klicka på nästa** tooadvance.
1. Nu väljer hello roller önskade tooinstall på den här datorn **funktioner Ledningsroll** och/eller **funktioner Arbetsrollen** och **Klicka på nästa**

    ![Azure Functions-Runtime Preview-installationsprogram - urval för Systemroll][3]

    > [!NOTE]
    > Du kan installera hello **funktioner Arbetsrollen** så, följ instruktionerna på många datorer toodo och välj endast **funktioner Arbetsrollen** i hello installer.

1. **Klicka på nästa** toohave hello **Azure Functions Runtime Installer** installera på datorn.
1. När fullständig hello installationsprogrammet startar hello **konfigurationsverktyget för Azure Functions-Runtime**.

    ![Azure Functions Runtime Preview Installer klar][5]

    > [!NOTE]
    > Om du installerar på **Windows 10** och hello **behållare** funktionen har inte tidigare har aktiverats, hello **Azure Functions-Runtime** uppmanas av installationsprogrammet tooreboot din dator toocomplete hello installera.

## <a name="configure-hello-azure-functions-runtime"></a>Konfigurera hello Azure Functions-Runtime

toocomplete hello Azure Functions-Runtime-installationen måste du slutföra hello konfiguration.

1. Hej **konfigurationsverktyget för Azure Functions-Runtime** visas vilka roller som är installerade på datorn.

    ![Azure Functions-Runtime Preview-konfigurationsverktyget][6]

1. Klicka på hello **databasen** ange hello **anslutningsinformationen för SQL Server-instans** och **på Verkställ**.  Detta är nödvändigt i ordning toohello Azure Functions-Runtime toocreate en databas toosupport hello Runtime.
    
    ![Azure Functions Runtime Preview databaskonfigurationen][7]

1. Klicka på hello **autentiseringsuppgifter** fliken.  På den här sidan måste du skapa två nya autentiseringsuppgifter för användning med en filresurs som värd för dina Azure-funktioner.  **Ange användarnamn och lösenord** kombinationer för hello **resursen filägaren** och för hello **filen resursen användaren** och klicka på **tillämpa**.

    ![Azure Functions-Runtime Preview autentiseringsuppgifter][8]

1. Klicka på hello **filresurs** fliken.  I den här skärmen måste du ange hello information om hello **filresurs plats**.  Detta kan skapas för dig eller Använd en befintlig resurs och på **tillämpa**.  Om du väljer en ny filresurs-plats måste du ange en katalog för användning av hello Azure Functions-Runtime.
    
    ![Azure Functions Runtime Preview filresurs][9]

1. Klicka på hello **IIS** fliken.  Den här fliken visar hello detaljerad information om hello webbplatser i IIS som hello Azure Functions Runtime installationen skapas.  **Klicka på Verkställ** toocomplete.

    ![Förhandsgranska Azure Functions-Runtime IIS][10]

1. Klicka på hello **Services** fliken.  Den här fliken visar hello status hello tjänster i Azure Functions-Runtime-installationen.  Om efter den inledande konfigurationen hello **Azure Functions värdtjänsten aktivering** körs inte klicka **starta tjänsten**

    ![Azure Functions Runtime Preview Configruation klar][11]

1. Slutligen Bläddra toohello **Azure Functions-Runtime portalen** som`https://<machinename>/`

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