---
title: "Hur du konfigurerar MSI på en virtuell Azure-dator med hjälp av Azure portal"
description: "Steg för steg-instruktioner för hur du konfigurerar en hanterad tjänst identitet (MSI) på en Azure VM som använder Azure portal."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: bryanla
ms.openlocfilehash: 8decfcedec94b9d78eac73a3e8db1219fac02029
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Konfigurera en virtuell dator hanteras Service identitet (MSI) med Azure-portalen

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Hanterade tjänstidentiteten ger Azure-tjänster med en automatiskt hanterade identitet i Azure Active Directory. Du kan använda den här identiteten för att autentisera till alla tjänster som stöder Azure AD-autentisering utan autentiseringsuppgifter i koden. 

I den här artikeln får du lära dig hur du aktiverar och ta bort MSI för en Azure-dator med hjälp av Azure portal.

## <a name="prerequisites"></a>Krav

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Aktivera MSI under skapandet av en Azure VM

Från och med när detta skrivs stöds aktivera MSI under skapandet av en virtuell dator i Azure portal inte. I stället finns på någon av följande VM skapa Quickstart artiklar först skapa en virtuell dator:

- [Skapa en Windows-dator med Azure-portalen](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Skapa en virtuell Linux-dator med Azure-portalen](../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Gå sedan vidare till nästa avsnitt för mer information om hur du aktiverar MSI på den virtuella datorn.

## <a name="enable-msi-on-an-existing-azure-vm"></a>Aktivera MSI på en befintlig virtuell Azure-dator

Om du har en virtuell dator som ursprungligen var etablerad utan en MSI:

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är associerade med Azure-prenumeration som innehåller den virtuella datorn. Kontrollera också att ditt konto hör till en roll som ger dig skrivbehörighet på den virtuella datorn, till exempel ”Virtual Machine-deltagare”.

2. Navigera till den önskade virtuella datorn.

2. Klicka på sidan ”Configuration”, aktivera MSI på den virtuella datorn genom att välja ”Ja” under ”hanterade tjänstidentiteten” och sedan på **spara**. Den här åtgärden kan ta 60 sekunder eller mer att slutföra:

   ![Konfiguration av sidan skärmbild](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>Ta bort MSI från en virtuell dator i Azure

Om du har en virtuell dator som inte längre behöver en MSI:

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är associerade med Azure-prenumeration som innehåller den virtuella datorn. Kontrollera också att ditt konto hör till en roll som ger dig skrivbehörighet på den virtuella datorn, till exempel ”Virtual Machine-deltagare”.

2. Navigera till den önskade virtuella datorn.

3. Klicka på sidan ”Configuration”, ta bort MSI från den virtuella datorn genom att välja ”Nej” under ”hanterade tjänstidentiteten” och sedan på **spara**. Den här åtgärden kan ta 60 sekunder eller mer att slutföra:

   ![Konfiguration av sidan skärmbild](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Relaterat innehåll

- En översikt över MSI finns [hanterade tjänstidentiteten översikt](msi-overview.md).

## <a name="next-steps"></a>Nästa steg

- Använder Azure-portalen ger en Azure VM MSI [åtkomst till en annan Azure-resurs](msi-howto-assign-access-portal.md).

Använd följande avsnitt för kommentarer för att ge feedback och hjälp oss att förfina och utforma innehållet.
