---
title: "Tekniska artiklar för klassiska virtuella Windows-datorer | Microsoft Azure"
description: "En fullständig lista över artiklar för Microsoft Azure-dokumentationen för Windows-datorer i den klassiska distributionsmodellen"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-service-management
editor: 
ms.assetid: 7f9a508b-34ec-4bdb-92d1-8844480369d5
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: danlep
ms.openlocfilehash: 2df7ea6a143ad0d64e4fd75223c7e5a9a2a5e87e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="technical-articles-for-windows-vms-in-the-classic-deployment-model"></a>Tekniska artiklar för virtuella Windows-datorer i den klassiska distributionsmodellen
Hitta all dokumentation som du behöver skapa och hantera Windows-baserade virtuella Azure-datorer i den klassiska distributionsmodellen.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker den klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.

## <a name="overview"></a>Översikt
[Om virtuella datorer](windows/overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Vanliga frågor om Azure virtuella datorer som skapats med den klassiska distributionsmodellen](windows/classic/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Jämför virtuella datorer, webbplatser och molntjänster](../app-service-web/choose-web-site-cloud-service-vm.md)

[Virtuella datorer och behållare i Azure](windows/containers.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="environment-setup"></a>Konfigurera miljön
[Kostnadsfritt konto](https://azure.microsoft.com/free/)

[Installera Azure PowerShell](/powershell/azure/overview)

[Installera Azure CLI](../cli-install-nodejs.md)

## <a name="get-started"></a>Kom igång
[Utbildningsväg för virtuella Windows-datorer](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)

[Skapa en virtuell Windows-dator i den klassiska Azure-portalen](windows/classic/tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Logga in till en klassisk virtuell dator som kör Windows Server](windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="plan"></a>Planera
[Om bilder för klassiska virtuella datorer](windows/classic/about-images.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Storlekar för virtuella datorer](windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Högpresterande beräkning VM-storlekar](windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Planerat underhåll för virtuella datorer i Azure](windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Vägledning för implementering av Azure-infrastrukturtjänster](windows/infrastructure-subscription-accounts-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Skapa en tillgänglighetsuppsättning för virtuella datorer](windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="deploy"></a>Distribuera
[Skapa en egen virtuell dator som kör Windows](windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Avbilda en Windows-dator som skapats i den klassiska distributionsmodellen](windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Skapa och ladda upp en klassiska Windows Server VHD med PowerShell](windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Automatisera distributionen av Azure virtuella datorer med Chef](windows/chef-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Skapa och konfigurera en klassiska Windows virtuell dator i Azure PowerShell](windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Placera anpassade data till en virtuell Azure-dator](windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="manage"></a>Hantera
[Hantera dina virtuella datorer med hjälp av Azure PowerShell](windows/classic/manage-psh.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Ansluta klassiska Vnet till nya Vnet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)

[Om den virtuella datorns agent och tillägg](windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Hantera tillägg för virtuell dator](windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Tillägget för anpassat skript för klassiska virtuella Windows-datorer](windows/classic/extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Plattform som stöds migreringen från klassiskt till Azure Resource Manager](windows/migration-classic-resource-manager-deep-dive.md)

## <a name="configure"></a>Konfigurera
[Hur du återställer ett lösenord eller tjänsten Remote Desktop för en virtuell Windows-dator](windows/reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Om virtuella datortillägg och funktioner](windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Hur du installerar och konfigurerar Symantec Endpoint Protection på en Windows VM](windows/classic/install-symantec.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Hur du installerar och konfigurerar Trend Micro djup Security som en tjänst på en Windows VM](windows/classic/install-trend.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Så här konfigurerar du en tillgänglighetsuppsättning för virtuella datorer i den klassiska distributionsmodellen](windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Hur du ställer in slutpunkter på klassiska Azure-dator](windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="storage"></a>Lagring
[Om diskar och virtuella hårddiskar för virtuella Azure-datorer](../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Så här kopplar du en datadisk till en klassiska Windows virtuell dator](windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Hur du koppla från en datadisk från en klassiska Windows virtuell dator](windows/classic/detach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Använda enhet D som en dataenhet på en Windows VM](windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="networking"></a>Nätverk
[Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md)

[Ansluta virtuella datorer som skapats med den klassiska distributionsmodellen med virtuella nätverk eller moln-tjänsten](windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Hantera NSG:er med hjälp av Azure PowerShell](../virtual-network/virtual-networks-create-nsg-classic-ps.md)

[Skapa en belastningsutjämnare](../load-balancer/load-balancer-get-started-internet-classic-portal.md)

## <a name="develop"></a>Utveckla
[Skapa och hantera virtuella Azure-datorer i Visual Studio](windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Skapa en virtuell dator för ett webbprogram med Visual Studio](windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Hur du kör en krävande uppgift i Java på en virtuell dator](windows/classic/java-run-compute-intensive-task.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Django Hello World webbprogram på en Windows Server-VM](windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="workloads"></a>Arbetsbelastningar
[HPC Pack](windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[MongoDB](windows/classic/install-mongodb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[MySQL](windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Oracle](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html#support)

[SAP](windows/classic/sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[SQL Server](./windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[tomcat](windows/classic/java-run-tomcat-app-server.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="reference"></a>Referens
[Azure CLI-kommandon i Service Management-läge](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)

[Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)

[Service Management .NET-API](https://msdn.microsoft.com/library/azure/mt420161.aspx)

[Azure Service Management PowerShell-cmdlet-referensdokumentationen](/powershell/azure/overview?view=azuresmps-3.7.0)

## <a name="troubleshooting"></a>Felsökning
[Felsöka fjärrskrivbordsanslutningar till en virtuell Azure-dator som kör Windows](windows/troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Felsök åtkomst till ett program som körs på en virtuell Azure-dator](windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Felsök tilldelningsproblem när du skapar, startar om eller ändra storlek på virtuella datorer i Azure](windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Felsöka klassisk distributionsproblem med att skapa en ny Windows virtuell dator i Azure](windows/classic/troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure](windows/classic/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
