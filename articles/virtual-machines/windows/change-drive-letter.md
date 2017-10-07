---
title: "Se hello D: enhet på en virtuell dator en datadisk | Microsoft Docs"
description: "Beskriver hur toochange enhetsbeteckningar för en virtuell Windows-dator så att du kan använda hello D: enhet som en dataenhet."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: 43939da1a890ac4049266487951e3889aa0ed9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-d-drive-as-a-data-drive-on-a-windows-vm"></a>Använda hello D: enheten som en dataenhet på en Windows VM
Om ditt program måste toouse hello D enhet toostore data, följer du dessa anvisningar toouse en annan enhetsbeteckning för hello diskutrymme. Använd aldrig hello tillfälliga toostore diskdata måste tookeep.

Om du ändrar storlek eller **stoppa (Deallocate)** en virtuell dator, detta kan utlösa placeringen av hello virtuella tooa nya hypervisor. En planerad eller oplanerad underhållshändelse kan också utlösa den här placering. I det här scenariot hello diskutrymme inte omtilldelas toohello första tillgängliga enhetsbeteckning. Om du har ett program som kräver mer specifikt hello D: enhet behöver du toofollow dessa steg tootemporarily flytta hello pagefile.sys ansluta en ny datadisk och tilldela den hello bokstaven D och flytta hello pagefile.sys tillbaka toohello tillfälliga enheten. När du är klar, träder Azure inte tillbaka hello D: om hello VM flyttas tooa olika hypervisor.

Mer information om hur Azure använder hello diskutrymme finns [förstå hello tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="attach-hello-data-disk"></a>Ansluta hello datadisk
Du måste först tooattach hello data disk toohello virtuella datorn. toodo detta med hjälp av hello portal, se [hur tooattach hanterade data disken i hello Azure-portalen](attach-managed-disk-portal.md).

## <a name="temporarily-move-pagefilesys-tooc-drive"></a>Tillfälligt flytta pagefile.sys tooC enhet
1. Ansluta toohello virtuella datorn. 
2. Högerklicka på hello **starta** -menyn och välj **System**.
3. Välj hello vänstra menyn **avancerade systeminställningar**.
4. I hello **prestanda** väljer **inställningar**.
5. Välj hello **Avancerat** fliken.
6. I hello **virtuellt minne** väljer **ändra**.
7. Välj hello **C** enhet och klicka sedan på **storleken** och klicka sedan på **ange**.
8. Välj hello **D** enhet och klicka sedan på **Ingen växlingsfil** och klicka sedan på **ange**.
9. Klicka på Använd. Du får en varning om att hello-datorn måste startas om för att hello ändringar tootake påverkar toobe.
10. Starta om hello virtuella datorn.

## <a name="change-hello-drive-letters"></a>Ändra hello enhetsbeteckningar
1. En gång Hej VM omstarter, logga in igen toohello VM.
2. Klicka på hello **starta** -menyn och skriv **diskmgmt.msc** och tryck på RETUR. Diskhantering startar.
3. Högerklicka på **D**hello tillfälliga lagringsenhet och välj **ändra enhetsbeteckning och sökvägar**.
4. Under enhetsbeteckning, väljer en ny enhet som **T** och klicka sedan på **OK**. 
5. Högerklicka på hello datadisk och välj **ändra enhetsbeteckning och sökvägar**.
6. Välj enhet under enhetsbeteckning **D** och klicka sedan på **OK**. 

## <a name="move-pagefilesys-back-toohello-temporary-storage-drive"></a>Flytta pagefile.sys tillbaka toohello tillfälliga lagringsenhet
1. Högerklicka på hello **starta** -menyn och välj **System**
2. Välj hello vänstra menyn **avancerade systeminställningar**.
3. I hello **prestanda** väljer **inställningar**.
4. Välj hello **Avancerat** fliken.
5. I hello **virtuellt minne** väljer **ändra**.
6. Välj hello OS enhet **C** och på **Ingen växlingsfil** och klicka sedan på **ange**.
7. Välj hello tillfällig lagringsenhet **T** och klicka sedan på **storleken** och klicka sedan på **ange**.
8. Klicka på **Använd**. Du får en varning om att hello-datorn måste startas om för att hello ändringar tootake påverkar toobe.
9. Starta om hello virtuella datorn.

## <a name="next-steps"></a>Nästa steg
* Du kan öka hello lagring tillgänglig tooyour virtuella datorn med [koppla en ytterligare datadisk](attach-managed-disk-portal.md).

