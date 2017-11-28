---
title: "aaaAzure distribution av virtuella datorer för SAP för Windows | Microsoft Docs"
description: "Lär dig hur toodeploy SAP-program på Windows-datorer i Azure."
services: virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 22aaa98c-bb9f-472f-b546-0b294e033cda
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 121e317afc6498a896959e58ebfbdcbef9432ef5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-on-windows-vms-in-azure"></a><span data-ttu-id="68e0e-103">Distribuera SAP på virtuella Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="68e0e-103">Deploy SAP on Windows VMs in Azure</span></span>
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:sap-dbms-guide.md (Azure Virtual Machines DBMS deployment for SAP on Windows)
[dbms-guide-2.1]:sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching for VMs and VHDs)
[dbms-guide-2.2]:sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID)
[dbms-guide-2.3]:sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Structure of a RDBMS deployment)
[dbms-guide-3]:sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (High availability and disaster recovery with Azure VMs)
[dbms-guide-5.5.1]:sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 and later)
[dbms-guide-5.5.2]:sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 and earlier releases)
[dbms-guide-5.6]:sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Using a SQL Server image from hello Azure Marketplace)
[dbms-guide-5.8]:sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (General SQL Server for SAP on Azure summary)
[dbms-guide-5]:sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Specifics tooSQL Server RDBMS)
[dbms-guide-8.4.1]:sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Storage configuration)
[dbms-guide-8.4.2]:sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Backup and restore)
[dbms-guide-8.4.3]:sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Performance considerations for backup and restore)
[dbms-guide-8.4.4]:sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Other)
[dbms-guide-900-sap-cache-server-on-premises]:sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:sap-deployment-guide.md (Azure Virtual Machines deployment for SAP on Windows)
[deployment-guide-2.2]:#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resources)
[deployment-guide-3.1.2]:#3688666f-281f-425b-a312-a77e7db2dfab (Deploying a VM by using a custom image)
[deployment-guide-3.2]:#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: Deploying a VM from hello Azure Marketplace for SAP)
[deployment-guide-3.3]:#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: Deploying a VM with a custom image for SAP)
[deployment-guide-3.4]:#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: Moving a VM from on-premises using a non-generalized Azure VHD with SAP)
[deployment-guide-3]:#b3253ee3-d63b-4d74-a49b-185e76c4088e (Deployment scenarios of VMs for SAP on Microsoft Azure)
[deployment-guide-4.1]:#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Deploying Azure PowerShell cmdlets)
[deployment-guide-4.2]:#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Download and import SAP-relevant PowerShell cmdlets)
[deployment-guide-4.3]:#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join a VM tooan on-premises domain - Windows only)
[deployment-guide-4.4.2]:#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Download, install, and enable hello Azure VM Agent)
[deployment-guide-4.5.1]:#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Configure hello Azure Enhanced Monitoring Extension for SAP)
[deployment-guide-5.1]:#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness check for Azure Enhanced Monitoring for SAP)
[deployment-guide-5.2]:#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Health check for hello Azure monitoring infrastructure)
[deployment-guide-5.3]:#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Troubleshooting Azure monitoring for SAP)

[deployment-guide-configure-monitoring-scenario-1]:#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure monitoring)
[deployment-guide-configure-proxy]:#baccae00-6f79-4307-ade4-40292ce4e02d (Configure hello proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:#564adb4f-5c95-4041-9616-6635e83a810b (Checks and troubleshooting for setting up end-to-end monitoring)

[deploy-template-cli]:../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:sap-get-started.md
[getting-started-dbms]:sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:classic/virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:classic/virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:classic/virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:classic/virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:classic/virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:classic/virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-windows-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:sap-planning-guide.md (Azure Virtual Machines planning and implementation for SAP on Windows)
[planning-guide-1.2]:sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources)
[planning-guide-11]:sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High availability and disaster recovery for SAP NetWeaver running on Azure Virtual Machines)
[planning-guide-11.4.1]:sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (High availability for SAP Application Servers)
[planning-guide-11.5]:sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Using Autostart for SAP instances)
[planning-guide-2.1]:sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Cloud-only - Virtual Machine deployments in Azure without dependencies on hello on-premises customer network)
[planning-guide-2.2]:sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises - Deployment of single or multiple SAP VMs in Azure fully integrated with hello on-premises network)
[planning-guide-3.1]:sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regions)
[planning-guide-3.2.1]:sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fault domains)
[planning-guide-3.2.2]:sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade domains)
[planning-guide-3.2.3]:sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure availability sets)
[planning-guide-3.2]:sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtual machines concept)
[planning-guide-3.3.2]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-5.1.1]:sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Moving a VM from on-premises tooAzure with a non-generalized disk)
[planning-guide-5.1.2]:sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Deploying a VM with a customer specific image)
[planning-guide-5.2.1]:sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Preparation for moving a VM from on-premises tooAzure with a non-generalized disk)
[planning-guide-5.2.2]:sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Preparation for deploying a VM with a customer specific image for SAP)
[planning-guide-5.2]:sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Preparing VMs with SAP for Azure)
[planning-guide-5.3.1]:sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Difference between an Azure disk and an Azure image)
[planning-guide-5.3.2]:sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Uploading a VHD from on-premises tooAzure)
[planning-guide-5.4.2]:sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Copying disks between Azure Storage accounts)
[planning-guide-5.5.1]:sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD structure for SAP deployments)
[planning-guide-5.5.3]:sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Setting automount for attached disks)
[planning-guide-7.1]:sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Single VM with SAP NetWeaver demo/training scenario)
[planning-guide-7]:sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Concepts of Cloud-Only deployment of SAP instances)
[planning-guide-9.1]:sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring Solution for SAP)
[planning-guide-azure-premium-storage]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and data disks)

[powershell-install-configure]:/powershell/azureps-cmdlets-docs
[resource-group-authoring-templates]:../../resource-group-authoring-templates.md
[resource-group-overview]:../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../storage/storage-premium-storage.md
[storage-redundancy]:../../storage/storage-redundancy.md
[storage-scalability-targets]:../../storage/storage-scalability-targets.md
[storage-use-azcopy]:../../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../linux/attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../linux/cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:../virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../linux/update-agent.md
[virtual-machines-manage-availability]:../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:classic/ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:classic/ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:upload-image.md
[virtual-machines-windows-tutorial]:../virtual-machines-windows-hero-tutorial.md
[virtual-machines-windows-create-vm-specialized]:../virtual-machines-windows-create-vm-specialized.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../xplat-cli-azure-resource-manager.md

<span data-ttu-id="68e0e-115">Virtuella Azure-datorer är hello lösning för organisationer som behöver resurser för beräkning och lagring, minimalt, och utan långa inköp cykler.</span><span class="sxs-lookup"><span data-stu-id="68e0e-115">Azure Virtual Machines is hello solution for organizations that need compute and storage resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="68e0e-116">Du kan använda Azure virtuella datorer toodeploy klassisk program, liksom SAP NetWeaver-baserade program i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-116">You can use Azure Virtual Machines toodeploy classical applications, like SAP NetWeaver-based applications, in Azure.</span></span> <span data-ttu-id="68e0e-117">Utöka tillförlitlighet och tillgänglighet utan ytterligare lokala resurser för ett program.</span><span class="sxs-lookup"><span data-stu-id="68e0e-117">Extend an application's reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="68e0e-118">Virtuella Azure-datorer stöder korsanslutningar, så du kan integrera Azure virtuella datorer i din organisations lokala domäner, privata moln och SAP system liggande.</span><span class="sxs-lookup"><span data-stu-id="68e0e-118">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="68e0e-119">I den här artikeln beskriver vi hello steg toodeploy SAP-program på Windows-datorer (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-119">In this article, we cover hello steps toodeploy SAP applications on Windows virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="68e0e-120">Den här artikeln bygger på hello informationen i [Azure Virtual Machines planering och implementering för SAP på Windows][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-120">This article builds on hello information in [Azure Virtual Machines planning and implementation for SAP on Windows][planning-guide].</span></span> <span data-ttu-id="68e0e-121">Den kompletterar SAP dokumentationen och SAP anteckningar, som är hello primära resurser för att installera och distribuera program.</span><span class="sxs-lookup"><span data-stu-id="68e0e-121">It also complements SAP installation documentation and SAP Notes, which are hello primary resources for installing and deploying SAP software.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68e0e-122">Krav</span><span class="sxs-lookup"><span data-stu-id="68e0e-122">Prerequisites</span></span>
<span data-ttu-id="68e0e-123">Konfigurera en virtuell Azure-dator för SAP programdistribution omfattar flera steg och resurser.</span><span class="sxs-lookup"><span data-stu-id="68e0e-123">Setting up an Azure virtual machine for SAP software deployment involves multiple steps and resources.</span></span> <span data-ttu-id="68e0e-124">Innan du börjar bör du kontrollera att du uppfyller hello krav för att installera program på Windows-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-124">Before you start, make sure that you meet hello prerequisites for installing SAP software on Windows virtual machines in Azure.</span></span>

### <a name="local-computer"></a><span data-ttu-id="68e0e-125">Lokal dator</span><span class="sxs-lookup"><span data-stu-id="68e0e-125">Local computer</span></span>
<span data-ttu-id="68e0e-126">toomanage Windows eller Linux-datorer som du kan använda ett PowerShell-skript och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-126">toomanage Windows or Linux VMs, you can use a PowerShell script and hello Azure portal.</span></span> <span data-ttu-id="68e0e-127">För båda verktygen behöver du en lokal dator som kör Windows 7 eller en senare version av Windows.</span><span class="sxs-lookup"><span data-stu-id="68e0e-127">For both tools, you need a local computer running Windows 7 or a later version of Windows.</span></span> <span data-ttu-id="68e0e-128">Om du vill toomanage endast virtuella Linux-datorer och du vill att toouse en Linux-dator för den här uppgiften kan du använda Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="68e0e-128">If you want toomanage only Linux VMs and you want toouse a Linux computer for this task, you can use Azure CLI.</span></span>

### <a name="internet-connection"></a><span data-ttu-id="68e0e-129">Internet-anslutning</span><span class="sxs-lookup"><span data-stu-id="68e0e-129">Internet connection</span></span>
<span data-ttu-id="68e0e-130">toodownload och kör hello verktyg och skript som krävs för SAP programdistribution, måste du vara ansluten toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-130">toodownload and run hello tools and scripts that are required for SAP software deployment, you must be connected toohello Internet.</span></span> <span data-ttu-id="68e0e-131">hello virtuella Azure-datorn som kör hello Azure förbättrad övervakning av tillägget för SAP måste också toohello för åtkomst till Internet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-131">hello Azure VM that is running hello Azure Enhanced Monitoring Extension for SAP also needs access toohello Internet.</span></span> <span data-ttu-id="68e0e-132">Om hello Azure VM är en del av ett virtuellt Azure-nätverk eller lokal domän, kontrollera att hello proxyinställningarna är inställda, enligt beskrivningen i [konfigurera hello proxy][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="68e0e-132">If hello Azure VM is part of an Azure virtual network or on-premises domain, make sure that hello relevant proxy settings are set, as described in [Configure hello proxy][deployment-guide-configure-proxy].</span></span>

### <a name="microsoft-azure-subscription"></a><span data-ttu-id="68e0e-133">Microsoft Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="68e0e-133">Microsoft Azure subscription</span></span>
<span data-ttu-id="68e0e-134">Du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="68e0e-134">You need an active Azure account.</span></span>

### <a name="topology-and-networking"></a><span data-ttu-id="68e0e-135">Topologi och nätverk</span><span class="sxs-lookup"><span data-stu-id="68e0e-135">Topology and networking</span></span>
<span data-ttu-id="68e0e-136">Du behöver toodefine hello topologi och arkitektur för hello SAP-distribution i Azure:</span><span class="sxs-lookup"><span data-stu-id="68e0e-136">You need toodefine hello topology and architecture of hello SAP deployment in Azure:</span></span>

* <span data-ttu-id="68e0e-137">Azure storage-konton toobe används</span><span class="sxs-lookup"><span data-stu-id="68e0e-137">Azure storage accounts toobe used</span></span>
* <span data-ttu-id="68e0e-138">Virtuellt nätverk där du vill att toodeploy hello SAP-systemet</span><span class="sxs-lookup"><span data-stu-id="68e0e-138">Virtual network where you want toodeploy hello SAP system</span></span>
* <span data-ttu-id="68e0e-139">Resurs grupp toowhich som du vill ha toodeploy hello SAP-system</span><span class="sxs-lookup"><span data-stu-id="68e0e-139">Resource group toowhich you want toodeploy hello SAP system</span></span>
* <span data-ttu-id="68e0e-140">Azure-region där du vill att toodeploy hello SAP-systemet</span><span class="sxs-lookup"><span data-stu-id="68e0e-140">Azure region where you want toodeploy hello SAP system</span></span>
* <span data-ttu-id="68e0e-141">SAP-konfiguration (två eller tre skikt)</span><span class="sxs-lookup"><span data-stu-id="68e0e-141">SAP configuration (two-tier or three-tier)</span></span>
* <span data-ttu-id="68e0e-142">VM-storlekar och hello antal ytterligare virtuella hårddiskar (VHD) toobe monterade toohello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="68e0e-142">VM sizes and hello number of additional virtual hard disks (VHDs) toobe mounted toohello VMs</span></span>
* <span data-ttu-id="68e0e-143">Konfiguration av SAP korrigering och Transport System (CTS)</span><span class="sxs-lookup"><span data-stu-id="68e0e-143">SAP Correction and Transport System (CTS) configuration</span></span>

<span data-ttu-id="68e0e-144">Skapa och konfigurera Azure storage-konton eller virtuella Azure-nätverk innan du börjar hello SAP process för programvarudistribution.</span><span class="sxs-lookup"><span data-stu-id="68e0e-144">Create and configure Azure storage accounts or Azure virtual networks before you begin hello SAP software deployment process.</span></span> <span data-ttu-id="68e0e-145">Information om hur toocreate och konfigurera dessa resurser, se [Azure Virtual Machines planering och implementering för SAP på Windows][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-145">For information about how toocreate and configure these resources, see [Azure Virtual Machines planning and implementation for SAP on Windows][planning-guide].</span></span>

### <a name="sap-sizing"></a><span data-ttu-id="68e0e-146">Storlek för SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-146">SAP sizing</span></span>
<span data-ttu-id="68e0e-147">Vet hello följande information för SAP storlek:</span><span class="sxs-lookup"><span data-stu-id="68e0e-147">Know hello following information, for SAP sizing:</span></span>

* <span data-ttu-id="68e0e-148">Planerat SAP arbetsbelastning, till exempel med hjälp av verktyget för hello SAP snabbt ange och hello SAP programmets prestanda Standard (SAP) tal</span><span class="sxs-lookup"><span data-stu-id="68e0e-148">Projected SAP workload, for example, by using hello SAP Quick Sizer tool, and hello SAP Application Performance Standard (SAPS) number</span></span>
* <span data-ttu-id="68e0e-149">Nödvändiga resurs och minne processorutnyttjande hello SAP-system</span><span class="sxs-lookup"><span data-stu-id="68e0e-149">Required CPU resource and memory consumption of hello SAP system</span></span>
* <span data-ttu-id="68e0e-150">Krävs för in-/ utdata (I/O)-åtgärder per sekund</span><span class="sxs-lookup"><span data-stu-id="68e0e-150">Required input/output (I/O) operations per second</span></span>
* <span data-ttu-id="68e0e-151">Krävs bandbredd på eventuell kommunikation mellan virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="68e0e-151">Required network bandwidth of eventual communication between VMs in Azure</span></span>
* <span data-ttu-id="68e0e-152">Nödvändiga nätverksbandbredden mellan lokala tillgångar och hello Azure distribuerade SAP-system</span><span class="sxs-lookup"><span data-stu-id="68e0e-152">Required network bandwidth between on-premises assets and hello Azure-deployed SAP system</span></span>

### <a name="resource-groups"></a><span data-ttu-id="68e0e-153">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="68e0e-153">Resource groups</span></span>
<span data-ttu-id="68e0e-154">I Azure Resource Manager, du kan använda resursen grupper toomanage alla hello-resurser i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="68e0e-154">In Azure Resource Manager, you can use resource groups toomanage all hello application resources in your Azure subscription.</span></span> <span data-ttu-id="68e0e-155">Mer information finns i [översikt över Azure Resource Manager][resource-group-overview].</span><span class="sxs-lookup"><span data-stu-id="68e0e-155">For more information, see [Azure Resource Manager overview][resource-group-overview].</span></span>

## <a name="resources"></a><span data-ttu-id="68e0e-156">Resurser</span><span class="sxs-lookup"><span data-stu-id="68e0e-156">Resources</span></span>

### <span data-ttu-id="68e0e-157"><a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-resurser</span><span class="sxs-lookup"><span data-stu-id="68e0e-157"><a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP resources</span></span>
<span data-ttu-id="68e0e-158">När du konfigurerar din SAP-programdistribution måste du hello följande SAP-resurser:</span><span class="sxs-lookup"><span data-stu-id="68e0e-158">When you are setting up your SAP software deployment, you need hello following SAP resources:</span></span>

* <span data-ttu-id="68e0e-159">SAP-kommentar [1928533], som innehåller:</span><span class="sxs-lookup"><span data-stu-id="68e0e-159">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="68e0e-160">Lista över Azure VM-storlekar som stöds för hello distributionen av program</span><span class="sxs-lookup"><span data-stu-id="68e0e-160">List of Azure VM sizes that are supported for hello deployment of SAP software</span></span>
  * <span data-ttu-id="68e0e-161">Viktiga kapacitetsinformation för Azure VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="68e0e-161">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="68e0e-162">Stöds SAP-programvara och operativsystem (OS) och kombinationer av databasen</span><span class="sxs-lookup"><span data-stu-id="68e0e-162">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="68e0e-163">SAP kernel version som krävs för Windows och Linux i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="68e0e-163">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="68e0e-164">SAP-kommentar [2015553] listar kraven för stöd för SAP SAP programvarudistributioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-164">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="68e0e-165">SAP-kommentar [2178632] innehåller detaljerad information om all övervakning mått som rapporterats för SAP i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-165">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="68e0e-166">SAP-kommentar [1409604] hello krävs SAP värden Agent-version för Windows i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-166">SAP Note [1409604] has hello required SAP Host Agent version for Windows in Azure.</span></span>
* <span data-ttu-id="68e0e-167">SAP-kommentar [2191498] hello krävs SAP värden Agent-version för Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-167">SAP Note [2191498] has hello required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="68e0e-168">SAP-kommentar [2243692] har licensieringsinformation SAP på Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-168">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="68e0e-169">SAP-kommentar [1984787] har allmän information om SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="68e0e-169">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="68e0e-170">SAP-kommentar [2002167] har allmän information om Red Hat Enterprise Linux 7.x.</span><span class="sxs-lookup"><span data-stu-id="68e0e-170">SAP Note [2002167] has general information about Red Hat Enterprise Linux 7.x.</span></span>
* <span data-ttu-id="68e0e-171">SAP-kommentar [1999351] innehåller ytterligare felsökningsinformation för hello Azure förbättrad övervakning av tillägget för SAP.</span><span class="sxs-lookup"><span data-stu-id="68e0e-171">SAP Note [1999351] has additional troubleshooting information for hello Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="68e0e-172">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) har alla nödvändiga SAP anteckningar för Linux.</span><span class="sxs-lookup"><span data-stu-id="68e0e-172">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="68e0e-173">SAP-specifika PowerShell-cmdletar som ingår i [Azure PowerShell][azure-ps].</span><span class="sxs-lookup"><span data-stu-id="68e0e-173">SAP-specific PowerShell cmdlets that are part of [Azure PowerShell][azure-ps].</span></span>
* <span data-ttu-id="68e0e-174">SAP-specifika Azure CLI-kommandona som ingår i [Azure CLI][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="68e0e-174">SAP-specific Azure CLI commands that are part of [Azure CLI][azure-cli].</span></span>

[comment]: <> (Lägg till MSSedusch TODO ARM korrigeringsnivå för SAP Värdagenten i SAP Obs 1409604)

### <a name="microsoft-resources"></a><span data-ttu-id="68e0e-176">Microsoft-resurser</span><span class="sxs-lookup"><span data-stu-id="68e0e-176">Microsoft resources</span></span>
<span data-ttu-id="68e0e-177">Dessa Microsoft-artiklarna omfatta SAP-distributioner i Azure:</span><span class="sxs-lookup"><span data-stu-id="68e0e-177">These Microsoft articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="68e0e-178">[Azure virtuella datorer planering och implementering för SAP i Windows][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="68e0e-178">[Azure Virtual Machines planning and implementation for SAP on Windows][planning-guide]</span></span>
* <span data-ttu-id="68e0e-179">[Distribution av Azure virtuella datorer för SAP på Windows (den här artikeln)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="68e0e-179">[Azure Virtual Machines deployment for SAP on Windows (this article)][deployment-guide]</span></span>
* <span data-ttu-id="68e0e-180">[Azure virtuella datorer DBMS-distribution för SAP i Windows][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="68e0e-180">[Azure Virtual Machines DBMS deployment for SAP on Windows][dbms-guide]</span></span>
* <span data-ttu-id="68e0e-181">[Azure-portalen][azure-portal]</span><span class="sxs-lookup"><span data-stu-id="68e0e-181">[Azure portal][azure-portal]</span></span>

## <span data-ttu-id="68e0e-182"><a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Distributionsscenarier för SAP-program på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="68e0e-182"><a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Deployment scenarios for SAP software on Azure VMs</span></span>
<span data-ttu-id="68e0e-183">Du har flera alternativ för att distribuera virtuella datorer och associerade diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-183">You have multiple options for deploying VMs and associated disks in Azure.</span></span> <span data-ttu-id="68e0e-184">Det är viktigt toounderstand hello skillnaderna mellan distributionsalternativen, eftersom du kan vidta olika steg tooprepare dina virtuella datorer för distribution baserad på hello-distributionstyp som du väljer.</span><span class="sxs-lookup"><span data-stu-id="68e0e-184">It's important toounderstand hello differences between deployment options, because you might take different steps tooprepare your VMs for deployment based on hello deployment type you choose.</span></span>

### <span data-ttu-id="68e0e-185"><a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Distribuera en virtuell dator från hello Azure Marketplace för SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-185"><a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Deploying a VM from hello Azure Marketplace for SAP</span></span>
<span data-ttu-id="68e0e-186">Du kan använda en avbildning som tillhandahålls av Microsoft eller av en tredje part i hello Azure Marketplace toodeploy den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-186">You can use an image provided by Microsoft or by a third party in hello Azure Marketplace toodeploy your VM.</span></span> <span data-ttu-id="68e0e-187">hello Marketplace erbjuder vissa standard operativsystemsavbildningar av Windows Server och olika Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="68e0e-187">hello Marketplace offers some standard OS images of Windows Server and different Linux distributions.</span></span> <span data-ttu-id="68e0e-188">Du kan också distribuera en avbildning som innehåller databashantering system (DBMS) SKU: er, till exempel Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="68e0e-188">You also can deploy an image that includes database management system (DBMS) SKUs, for example, Microsoft SQL Server.</span></span> <span data-ttu-id="68e0e-189">Mer information om hur du använder bilder med DBMS SKU: er finns [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-189">For more information about using images with DBMS SKUs, see [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide].</span></span>

<span data-ttu-id="68e0e-190">hello visar följande flödesschema hello SAP-specifika sekvens med stegen för att distribuera en virtuell dator från hello Azure Marketplace:</span><span class="sxs-lookup"><span data-stu-id="68e0e-190">hello following flowchart shows hello SAP-specific sequence of steps for deploying a VM from hello Azure Marketplace:</span></span>

![Flödesdiagram för distribution av Virtuella datorer för SAP-system med hjälp av en VM-avbildning från hello Azure Marketplace][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-hello-azure-portal"></a><span data-ttu-id="68e0e-192">Skapa en virtuell dator med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="68e0e-192">Create a virtual machine by using hello Azure portal</span></span>
<span data-ttu-id="68e0e-193">hello enklaste sättet toocreate en ny virtuell dator med en avbildning från hello Azure Marketplace är med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-193">hello easiest way toocreate a new virtual machine with an image from hello Azure Marketplace is by using hello Azure portal.</span></span>

1.  <span data-ttu-id="68e0e-194">Gå för<https://portal.azure.com/#create>.</span><span class="sxs-lookup"><span data-stu-id="68e0e-194">Go too<https://portal.azure.com/#create>.</span></span>  <span data-ttu-id="68e0e-195">Välj i hello Azure portal meny **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-195">Or, in hello Azure portal menu, select **+ New**.</span></span>
2.  <span data-ttu-id="68e0e-196">Välj **Compute**, och välj sedan hello typ av operativsystem som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="68e0e-196">Select **Compute**, and then select hello type of operating system you want toodeploy.</span></span> <span data-ttu-id="68e0e-197">Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12) eller Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span><span class="sxs-lookup"><span data-stu-id="68e0e-197">For example, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), or Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span></span> <span data-ttu-id="68e0e-198">hello standardvyn lista visar inte alla operativsystem som stöds.</span><span class="sxs-lookup"><span data-stu-id="68e0e-198">hello default list view does not show all supported operating systems.</span></span> <span data-ttu-id="68e0e-199">Välj **se alla** för en fullständig lista.</span><span class="sxs-lookup"><span data-stu-id="68e0e-199">Select **see all** for a full list.</span></span> <span data-ttu-id="68e0e-200">Mer information om operativsystem som stöds för SAP programdistribution finns SAP-kommentar [1928533].</span><span class="sxs-lookup"><span data-stu-id="68e0e-200">For more information about supported operating systems for SAP software deployment, see SAP Note [1928533].</span></span>
3.  <span data-ttu-id="68e0e-201">Granska villkoren på hello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="68e0e-201">On hello next page, review terms and conditions.</span></span>
4.  <span data-ttu-id="68e0e-202">I hello **Välj en distributionsmodell** väljer **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-202">In hello **Select a deployment model** box, select **Resource Manager**.</span></span>
5.  <span data-ttu-id="68e0e-203">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-203">Select **Create**.</span></span>

<span data-ttu-id="68e0e-204">hello-guiden hjälper dig att ange hello krävs parametrar toocreate hello virtuell dator, förutom tooall krävs resurser, t.ex. nätverksgränssnitt och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="68e0e-204">hello wizard guides you through setting hello required parameters toocreate hello virtual machine, in addition tooall required resources, like network interfaces and storage accounts.</span></span> <span data-ttu-id="68e0e-205">Några av dessa parametrar är:</span><span class="sxs-lookup"><span data-stu-id="68e0e-205">Some of these parameters are:</span></span>

1. <span data-ttu-id="68e0e-206">**Grunderna**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-206">**Basics**:</span></span>
  * <span data-ttu-id="68e0e-207">**Namnet**: hello namnet på hello resurs (hello virtuella namn).</span><span class="sxs-lookup"><span data-stu-id="68e0e-207">**Name**: hello name of hello resource (hello virtual machine name).</span></span>
 * <span data-ttu-id="68e0e-208">**Användarnamn och lösenord** eller **offentliga SSH-nyckeln**: Ange hello användarnamn och lösenord för hello-användare som har skapats under hello etablering.</span><span class="sxs-lookup"><span data-stu-id="68e0e-208">**Username and password** or **SSH public key**: Enter hello username and password of hello user that is created during hello provisioning.</span></span> <span data-ttu-id="68e0e-209">För en virtuell Linux-dator, kan du ange hello offentlig SSH (Secure Shell) nyckel som du använder toosign i toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-209">For a Linux virtual machine, you can enter hello public Secure Shell (SSH) key that you use toosign in toohello machine.</span></span>
 * <span data-ttu-id="68e0e-210">**Prenumerationen**: Välj hello prenumeration som du vill toouse tooprovision hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="68e0e-210">**Subscription**: Select hello subscription that you want toouse tooprovision hello new virtual machine.</span></span>
 * <span data-ttu-id="68e0e-211">**Resursgruppen**: hello namnet på resursgruppen hello för hello VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-211">**Resource group**: hello name of hello resource group for hello VM.</span></span> <span data-ttu-id="68e0e-212">Du kan ange antingen hello-namnet på en ny grupp eller hello resursnamnet för en resursgrupp som redan finns.</span><span class="sxs-lookup"><span data-stu-id="68e0e-212">You can enter either hello name of a new resource group or hello name of a resource group that already exists.</span></span>
 * <span data-ttu-id="68e0e-213">**Plats**: där toodeploy hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="68e0e-213">**Location**: Where toodeploy hello new virtual machine.</span></span> <span data-ttu-id="68e0e-214">Om du vill tooconnect hello virtuella tooyour lokalt nätverk, kontrollera att du väljer hello platsen för hello virtuella nätverk som ansluter Azure tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="68e0e-214">If you want tooconnect hello virtual machine tooyour on-premises network, make sure you select hello location of hello virtual network that connects Azure tooyour on-premises network.</span></span> <span data-ttu-id="68e0e-215">Mer information finns i [Microsoft Azure-nätverk] [ planning-guide-microsoft-azure-networking] i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-215">For more information, see [Microsoft Azure networking][planning-guide-microsoft-azure-networking] in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide].</span></span>
2. <span data-ttu-id="68e0e-216">**Storlek**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-216">**Size**:</span></span>

     <span data-ttu-id="68e0e-217">En lista över VM-typer som stöds finns i SAP-kommentar [1928533].</span><span class="sxs-lookup"><span data-stu-id="68e0e-217">For a list of supported VM types, see SAP Note [1928533].</span></span> <span data-ttu-id="68e0e-218">Måste du markera hello rätt VM-typ. Om du vill toouse Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="68e0e-218">Be sure you select hello correct VM type if you want toouse Azure Premium Storage.</span></span> <span data-ttu-id="68e0e-219">Inte alla VM-typer stöder Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="68e0e-219">Not all VM types support Premium Storage.</span></span> <span data-ttu-id="68e0e-220">Mer information finns i [lagring: Microsoft Azure Storage- och datadiskar] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] och [Azure Premium Storage] [ planning-guide-azure-premium-storage] i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-220">For more information, see [Storage: Microsoft Azure Storage and data disks][planning-guide-storage-microsoft-azure-storage-and-data-disks] and [Azure Premium Storage][planning-guide-azure-premium-storage] in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide].</span></span>

3. <span data-ttu-id="68e0e-221">**Inställningar för**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-221">**Settings**:</span></span>
   * <span data-ttu-id="68e0e-222">**Lagringskontot**: Välj ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="68e0e-222">**Storage account**: Select an existing storage account or create a new one.</span></span> <span data-ttu-id="68e0e-223">Inte alla lagringstyper av fungerar för SAP-program som körs.</span><span class="sxs-lookup"><span data-stu-id="68e0e-223">Not all storage types work for running SAP applications.</span></span> <span data-ttu-id="68e0e-224">Mer information om lagringstyper finns [Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-224">For more information about storage types, see [Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide].</span></span>
   * <span data-ttu-id="68e0e-225">**Virtuellt nätverk** och **undernät**: toointegrate hello virtuell dator med ditt intranät, Välj hello virtuellt nätverk som är anslutna tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="68e0e-225">**Virtual network** and **Subnet**: toointegrate hello virtual machine with your intranet, select hello virtual network that is connected tooyour on-premises network.</span></span>
   * <span data-ttu-id="68e0e-226">**Offentliga IP-adressen**: Välj hello offentlig IP-adress som du vill toouse eller ange parametrar toocreate en ny offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="68e0e-226">**Public IP address**: Select hello public IP address that you want toouse, or enter parameters toocreate a new public IP address.</span></span> <span data-ttu-id="68e0e-227">Du kan använda en offentlig IP-adress tooaccess din virtuella dator över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-227">You can use a public IP address tooaccess your virtual machine over hello Internet.</span></span> <span data-ttu-id="68e0e-228">Kontrollera att du också skapa en network security group toohelp säker åtkomst tooyour virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="68e0e-228">Make sure that you also create a network security group toohelp secure access tooyour virtual machine.</span></span>
   * <span data-ttu-id="68e0e-229">**Nätverkssäkerhetsgruppen**: Mer information finns i [styr flödet i nätverkstrafiken med nätverkssäkerhetsgrupper][virtual-networks-nsg].</span><span class="sxs-lookup"><span data-stu-id="68e0e-229">**Network security group**: For more information, see [Control network traffic flow with network security groups][virtual-networks-nsg].</span></span>
   * <span data-ttu-id="68e0e-230">**Tillgänglighet**: Välj en tillgänglighetsuppsättning eller ange hello parametrar toocreate en ny tillgänglighet anges.</span><span class="sxs-lookup"><span data-stu-id="68e0e-230">**Availability**: Select an availability set, or enter hello parameters toocreate a new availability set.</span></span> <span data-ttu-id="68e0e-231">Mer information finns i [Azure tillgänglighetsuppsättningar][planning-guide-3.2.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-231">For more information, see [Azure availability sets][planning-guide-3.2.3].</span></span>
   * <span data-ttu-id="68e0e-232">**Övervaka**: du kan välja **inaktivera** för övervakning av diagnostik.</span><span class="sxs-lookup"><span data-stu-id="68e0e-232">**Monitoring**: You can select **Disable** for monitoring diagnostics.</span></span> <span data-ttu-id="68e0e-233">Den aktiveras automatiskt när du kör hello kommandon tooenable hello Azure förbättrad övervakning tillägg, enligt beskrivningen i [Konfigurera övervakning][deployment-guide-configure-monitoring-scenario-1].</span><span class="sxs-lookup"><span data-stu-id="68e0e-233">It is enabled automatically when you run hello commands tooenable hello Azure Enhanced Monitoring Extension, as described in [Configure monitoring][deployment-guide-configure-monitoring-scenario-1].</span></span>

4. <span data-ttu-id="68e0e-234">**Sammanfattning**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-234">**Summary**:</span></span>

  <span data-ttu-id="68e0e-235">Granska dina val och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-235">Review your selections, and then select **OK**.</span></span>

<span data-ttu-id="68e0e-236">Den virtuella datorn distribueras i hello resursgrupp som du har valt.</span><span class="sxs-lookup"><span data-stu-id="68e0e-236">Your virtual machine is deployed in hello resource group you selected.</span></span>

#### <a name="create-a-virtual-machine-by-using-a-template"></a><span data-ttu-id="68e0e-237">Skapa en virtuell dator med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="68e0e-237">Create a virtual machine by using a template</span></span>
<span data-ttu-id="68e0e-238">Du kan skapa en virtuell dator med hjälp av en av hello SAP mallarna publicerats i hello [GitHub-lagringsplatsen för azure-snabbstartsmallar][azure-quickstart-templates-github].</span><span class="sxs-lookup"><span data-stu-id="68e0e-238">You can create a virtual machine by using one of hello SAP templates published in hello [azure-quickstart-templates GitHub repository][azure-quickstart-templates-github].</span></span> <span data-ttu-id="68e0e-239">Du kan också manuellt skapa en virtuell dator med hjälp av hello [Azure-portalen][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], eller [Azure CLI ][virtual-machines-linux-tutorial].</span><span class="sxs-lookup"><span data-stu-id="68e0e-239">You also can manually create a virtual machine by using hello [Azure portal][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], or [Azure CLI][virtual-machines-linux-tutorial].</span></span>

* <span data-ttu-id="68e0e-240">[**Tvålagers-(endast en virtuell dator) konfigurationsmallen** (sap-2-nivå-marketplace-avbildning)][sap-templates-2-tier-marketplace-image]</span><span class="sxs-lookup"><span data-stu-id="68e0e-240">[**Two-tier configuration (only one virtual machine) template** (sap-2-tier-marketplace-image)][sap-templates-2-tier-marketplace-image]</span></span>

  <span data-ttu-id="68e0e-241">toocreate system på två nivåer med bara en virtuell dator kan använda den här mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-241">toocreate a two-tier system by using only one virtual machine, use this template.</span></span>
* <span data-ttu-id="68e0e-242">[**Trelagers-(flera virtuella datorer) konfigurationsmallen** (sap-3-nivå-marketplace-avbildning)][sap-templates-3-tier-marketplace-image]</span><span class="sxs-lookup"><span data-stu-id="68e0e-242">[**Three-tier configuration (multiple virtual machines) template** (sap-3-tier-marketplace-image)][sap-templates-3-tier-marketplace-image]</span></span>

  <span data-ttu-id="68e0e-243">toocreate en trelagers-system med flera virtuella datorer och använda den här mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-243">toocreate a three-tier system by using multiple virtual machines, use this template.</span></span>

<span data-ttu-id="68e0e-244">Ange följande parametrar för hello mallen hello i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="68e0e-244">In hello Azure portal, enter hello following parameters for hello template:</span></span>

1. <span data-ttu-id="68e0e-245">**Grunderna**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-245">**Basics**:</span></span>
  * <span data-ttu-id="68e0e-246">**Prenumerationen**: hello prenumeration toouse toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-246">**Subscription**: hello subscription toouse toodeploy hello template.</span></span>
  * <span data-ttu-id="68e0e-247">**Resursgruppen**: hello resource group toouse toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-247">**Resource group**: hello resource group toouse toodeploy hello template.</span></span> <span data-ttu-id="68e0e-248">Du kan skapa en ny resursgrupp eller du kan välja en befintlig resursgrupp i hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="68e0e-248">You can create a new resource group, or you can select an existing resource group in hello subscription.</span></span>
  * <span data-ttu-id="68e0e-249">**Plats**: där toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-249">**Location**: Where toodeploy hello template.</span></span> <span data-ttu-id="68e0e-250">Om du har valt en befintlig resursgrupp används hello plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-250">If you selected an existing resource group, hello location of that resource group is used.</span></span>

2. <span data-ttu-id="68e0e-251">**Inställningar för**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-251">**Settings**:</span></span>
  * <span data-ttu-id="68e0e-252">**System-ID för SAP**: hello SAP System-ID (SID).</span><span class="sxs-lookup"><span data-stu-id="68e0e-252">**SAP System ID**: hello SAP System ID (SID).</span></span>
  * <span data-ttu-id="68e0e-253">**OS-typen**: hello operativsystem som du vill toodeploy, till exempel, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12) eller Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span><span class="sxs-lookup"><span data-stu-id="68e0e-253">**OS type**: hello operating system you want toodeploy, for example, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), or Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span></span>

    <span data-ttu-id="68e0e-254">hello standardvyn lista visar inte alla operativsystem som stöds.</span><span class="sxs-lookup"><span data-stu-id="68e0e-254">hello default list view does not show all supported operating systems.</span></span> <span data-ttu-id="68e0e-255">Välj **se alla** för en fullständig lista.</span><span class="sxs-lookup"><span data-stu-id="68e0e-255">Select **see all** for a full list.</span></span> <span data-ttu-id="68e0e-256">Mer information om operativsystem som stöds för SAP programdistribution finns SAP-kommentar [1928533].</span><span class="sxs-lookup"><span data-stu-id="68e0e-256">For more information about supported operating systems for SAP software deployment, see SAP Note [1928533].</span></span>
  * <span data-ttu-id="68e0e-257">**SAP systemstorlek**: hello storleken på hello SAP-system.</span><span class="sxs-lookup"><span data-stu-id="68e0e-257">**SAP system size**: hello size of hello SAP system.</span></span>

    <span data-ttu-id="68e0e-258">hello antalet SAP hello nya system ger.</span><span class="sxs-lookup"><span data-stu-id="68e0e-258">hello number of SAPS hello new system provides.</span></span> <span data-ttu-id="68e0e-259">Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.</span><span class="sxs-lookup"><span data-stu-id="68e0e-259">If you are not sure how many SAPS hello system requires, ask your SAP Technology Partner or System Integrator.</span></span>
  * <span data-ttu-id="68e0e-260">**Filsystemets tillgänglighet** (endast tre skikt mall): hello filsystemets tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-260">**System availability** (three-tier template only): hello system availability.</span></span>

    <span data-ttu-id="68e0e-261">Välj **HA** för en konfiguration som passar för en installation med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-261">Select **HA** for a configuration that is suitable for a high-availability installation.</span></span> <span data-ttu-id="68e0e-262">Två databasservrar och två servrar för ABAP SAP centrala tjänster (ASCS) skapas.</span><span class="sxs-lookup"><span data-stu-id="68e0e-262">Two database servers and two servers for ABAP SAP Central Services (ASCS) are created.</span></span>
  * <span data-ttu-id="68e0e-263">**Lagringstyp** (endast två nivåer mall): hello typ av lagring toouse.</span><span class="sxs-lookup"><span data-stu-id="68e0e-263">**Storage type** (two-tier template only): hello type of storage toouse.</span></span>

    <span data-ttu-id="68e0e-264">Vi rekommenderar med Azure Premium-lagring för större system.</span><span class="sxs-lookup"><span data-stu-id="68e0e-264">For larger systems, we highly recommend using Azure Premium Storage.</span></span> <span data-ttu-id="68e0e-265">Mer information om lagringstyper finns i följande källor:</span><span class="sxs-lookup"><span data-stu-id="68e0e-265">For more information about storage types, see these resources:</span></span>
      * <span data-ttu-id="68e0e-266">[Användning av Azure Premium SSD-lagring för SAP DBMS-instans][2367194]</span><span class="sxs-lookup"><span data-stu-id="68e0e-266">[Use of Azure Premium SSD Storage for SAP DBMS Instance][2367194]</span></span>
      * <span data-ttu-id="68e0e-267">[Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="68e0e-267">[Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
      * <span data-ttu-id="68e0e-268">[Premium-lagring: Högpresterande lagring för arbetsbelastningar på Azure-dator][storage-premium-storage-preview-portal]</span><span class="sxs-lookup"><span data-stu-id="68e0e-268">[Premium Storage: High-performance storage for Azure Virtual Machine workloads][storage-premium-storage-preview-portal]</span></span>
      * <span data-ttu-id="68e0e-269">[Introduktion tooMicrosoft Azure Storage][storage-introduction]</span><span class="sxs-lookup"><span data-stu-id="68e0e-269">[Introduction tooMicrosoft Azure Storage][storage-introduction]</span></span>
  * <span data-ttu-id="68e0e-270">**Administratörsanvändarnamnet** och **adminlösenord**: ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="68e0e-270">**Admin username** and **Admin password**: A username and password.</span></span>
    <span data-ttu-id="68e0e-271">En ny användare skapas för att logga in toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-271">A new user is created, for signing in toohello virtual machine.</span></span>
  * <span data-ttu-id="68e0e-272">**Ny eller befintlig undernät**: Anger om skapas ett nytt virtuellt nätverk och undernät eller ett befintligt undernät används.</span><span class="sxs-lookup"><span data-stu-id="68e0e-272">**New or existing subnet**: Determines whether a new virtual network and subnet are  created or an existing subnet is used.</span></span> <span data-ttu-id="68e0e-273">Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-273">If you already have a virtual network that is connected tooyour on-premises network, select **Existing**.</span></span>
  * <span data-ttu-id="68e0e-274">**Undernät-ID**: hello-ID för virtuella datorer i hello undernät hello ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="68e0e-274">**Subnet ID**: hello ID of hello subnet hello virtual machines will connect to.</span></span> <span data-ttu-id="68e0e-275">Välj hello undernät för virtuellt privat nätverk (VPN) eller Azure ExpressRoute virtuellt nätverk toouse tooconnect hello tooyour lokalt nätverk för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="68e0e-275">Select hello subnet of your virtual private network (VPN) or Azure ExpressRoute virtual network toouse tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="68e0e-276">hello ID ser oftast ut så här: /subscriptions/&lt;prenumerations-id > /resourceGroups/&lt;resursgruppens namn > /providers/Microsoft.Network/virtualNetworks/&lt;virtuella nätverksnamnet > /subnets/&lt;undernätsnamn ></span><span class="sxs-lookup"><span data-stu-id="68e0e-276">hello ID usually looks like this: /subscriptions/&lt;subscription id>/resourceGroups/&lt;resource group name>/providers/Microsoft.Network/virtualNetworks/&lt;virtual network name>/subnets/&lt;subnet name></span></span>

3. <span data-ttu-id="68e0e-277">**Villkor**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-277">**Terms and conditions**:</span></span>  
    <span data-ttu-id="68e0e-278">Granska och Godkänn hello juridiska villkor.</span><span class="sxs-lookup"><span data-stu-id="68e0e-278">Review and accept hello legal terms.</span></span>

4.  <span data-ttu-id="68e0e-279">Välj **inköp**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-279">Select **Purchase**.</span></span>

<span data-ttu-id="68e0e-280">hello Azure VM-agenten distribueras som standard när du använder en bild från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="68e0e-280">hello Azure VM Agent is deployed by default when you use an image from hello Azure Marketplace.</span></span>

#### <a name="configure-proxy-settings"></a><span data-ttu-id="68e0e-281">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="68e0e-281">Configure proxy settings</span></span>
<span data-ttu-id="68e0e-282">Beroende på hur det lokala nätverket är konfigurerat kan behöva du tooset in hello proxy på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-282">Depending on how your on-premises network is configured, you might need tooset up hello proxy on your VM.</span></span> <span data-ttu-id="68e0e-283">Om den virtuella datorn är ansluten tooyour lokalt nätverk via VPN eller ExpressRoute, kan hello VM inte vara kan tooaccess hello Internet, och inte kan toodownload hello krävs tillägg eller samla in övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="68e0e-283">If your VM is connected tooyour on-premises network via VPN or ExpressRoute, hello VM might not be able tooaccess hello Internet, and won't be able toodownload hello required extensions or collect monitoring data.</span></span> <span data-ttu-id="68e0e-284">Mer information finns i [konfigurera hello proxy][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="68e0e-284">For more information, see [Configure hello proxy][deployment-guide-configure-proxy].</span></span>

#### <a name="join-a-domain-windows-only"></a><span data-ttu-id="68e0e-285">Anslut till en domän (endast Windows)</span><span class="sxs-lookup"><span data-stu-id="68e0e-285">Join a domain (Windows only)</span></span>
<span data-ttu-id="68e0e-286">Om din Azure-distribution är anslutna tooan lokala Active Directory DNS-instans eller via en Azure-plats-till-plats VPN-anslutning eller ExpressRoute (Detta kallas *mellan lokala* i [planering av virtuella datorer i Azure och implementeringen för SAP på Linux][planning-guide]), det förväntas att hello VM är ansluter till en lokal domän.</span><span class="sxs-lookup"><span data-stu-id="68e0e-286">If your Azure deployment is connected tooan on-premises Active Directory or DNS instance via an Azure site-to-site VPN connection or ExpressRoute (this is called *cross-premises* in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), it is expected that hello VM is joining an on-premises domain.</span></span> <span data-ttu-id="68e0e-287">Mer information om överväganden för den här aktiviteten finns [ansluta till en VM tooan lokal domän (Windows)][deployment-guide-4.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-287">For more information about considerations for this task, see [Join a VM tooan on-premises domain (Windows only)][deployment-guide-4.3].</span></span>

#### <span data-ttu-id="68e0e-288"><a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Konfigurera övervakning</span><span class="sxs-lookup"><span data-stu-id="68e0e-288"><a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Configure monitoring</span></span>
<span data-ttu-id="68e0e-289">toobe att din miljö har stöd för SAP, ställa in hello Azure övervakning tillägget för SAP som beskrivs i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="68e0e-289">toobe sure your environment supports SAP, set up hello Azure Monitoring Extension for SAP as described in [Configure hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="68e0e-290">Kontrollera hello krav för SAP övervakning och minsta operativsystemversioner som krävs SAP Kernel och SAP Värdagenten hello resurser som anges i [SAP resurser][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-290">Check hello prerequisites for SAP monitoring, and required minimum versions of SAP Kernel and SAP Host Agent, in hello resources listed in [SAP resources][deployment-guide-2.2].</span></span>

#### <a name="monitoring-check"></a><span data-ttu-id="68e0e-291">Övervakning av kontrollen</span><span class="sxs-lookup"><span data-stu-id="68e0e-291">Monitoring check</span></span>
<span data-ttu-id="68e0e-292">Kontrollera om övervakning fungerar, enligt beskrivningen i [kontroller och felsökning för att ställa in slutpunkt till slutpunkt övervakning][deployment-guide-troubleshooting-chapter].</span><span class="sxs-lookup"><span data-stu-id="68e0e-292">Check whether monitoring is working, as described in [Checks and troubleshooting for setting up end-to-end monitoring][deployment-guide-troubleshooting-chapter].</span></span>

#### <a name="post-deployment-steps"></a><span data-ttu-id="68e0e-293">Efter distributionen steg</span><span class="sxs-lookup"><span data-stu-id="68e0e-293">Post-deployment steps</span></span>
<span data-ttu-id="68e0e-294">När du har skapat hello VM och hello VM distribueras måste du tooinstall hello krävs programvarukomponenter i hello VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-294">After you create hello VM and hello VM is deployed, you need tooinstall hello required software components in hello VM.</span></span> <span data-ttu-id="68e0e-295">På grund av hello distribution och programvara installationen sekvens i den här typen av distribution av Virtuella datorer, måste hello programvara toobe installerad redan finnas, antingen i Azure, på en annan virtuell dator eller som en disk som kan bifogas.</span><span class="sxs-lookup"><span data-stu-id="68e0e-295">Because of hello deployment/software installation sequence in this type of VM deployment, hello software toobe installed must already be available, either in Azure, on another VM, or as a disk that can be attached.</span></span> <span data-ttu-id="68e0e-296">Eller, med hjälp av en scenarion med flera lokaler, i vilken anslutningen toohello lokala tillgångar (Installationsresurser) anges.</span><span class="sxs-lookup"><span data-stu-id="68e0e-296">Or, consider using a cross-premises scenario, in which connectivity toohello on-premises assets (installation shares) is given.</span></span>

<span data-ttu-id="68e0e-297">När du har distribuerat den virtuella datorn i Azure hello följa samma riktlinjer och verktyg tooinstall hello SAP-program på den virtuella datorn som du skulle ha gjort i en lokal miljö.</span><span class="sxs-lookup"><span data-stu-id="68e0e-297">After you deploy your VM in Azure, follow hello same guidelines and tools tooinstall hello SAP software on your VM as you would in an on-premises environment.</span></span> <span data-ttu-id="68e0e-298">tooinstall SAP-program på en Azure VM, både SAP och Microsoft rekommenderar att du överför och lagra hello SAP-installationsmediet på Azure virtuella hårddiskar eller att du skapar en virtuell Azure-dator som fungerar som en filserver som har alla hello krävs för SAP-installationsmediet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-298">tooinstall SAP software on an Azure VM, both SAP and Microsoft recommend that you upload and store hello SAP installation media on Azure VHDs, or that you create an Azure VM that works as a file server that has all hello required SAP installation media.</span></span>

[comment]: <> (MSSedusch TODO varför vi behöver toorecommend filhantering, till exempel filserver eller VHD? Skiljer sig som så från lokala?)

### <span data-ttu-id="68e0e-300"><a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Distribuera en virtuell dator med en anpassad avbildning för SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-300"><a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Deploying a VM with a custom image for SAP</span></span>
<span data-ttu-id="68e0e-301">Eftersom olika versioner av ett operativsystem eller DBMS har olika krav, uppfyller hello-avbildningar som du hittar i hello Azure Marketplace inte dina behov.</span><span class="sxs-lookup"><span data-stu-id="68e0e-301">Because different versions of an operating system or DBMS have different patch requirements, hello images you find in hello Azure Marketplace might not meet your needs.</span></span> <span data-ttu-id="68e0e-302">I stället kanske du vill toocreate en virtuell dator med hjälp av en egen OS/DBMS VM-avbildning som du kan distribuera igen senare.</span><span class="sxs-lookup"><span data-stu-id="68e0e-302">You might instead want toocreate a VM by using your own OS/DBMS VM image, which you can deploy again later.</span></span>
<span data-ttu-id="68e0e-303">Du kan använda olika steg toocreate en privat avbildning för Linux än toocreate ett för Windows.</span><span class="sxs-lookup"><span data-stu-id="68e0e-303">You use different steps toocreate a private image for Linux than toocreate one for Windows.</span></span>

- - -
> ![Windows][Logo_Windows] <span data-ttu-id="68e0e-305">Windows</span><span class="sxs-lookup"><span data-stu-id="68e0e-305">Windows</span></span>
>
> <span data-ttu-id="68e0e-306">tooprepare en Windows-avbildning som du kan använda toodeploy flera virtuella datorer, hello Windows-inställningar (t.ex. Windows-SID och värdnamn) måste tas ut eller generaliserad på hello lokalt VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-306">tooprepare a Windows image that you can use toodeploy multiple virtual machines, hello Windows settings (like Windows SID and hostname) must be abstracted or generalized on hello on-premises VM.</span></span> <span data-ttu-id="68e0e-307">Du kan använda [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) toodo detta.</span><span class="sxs-lookup"><span data-stu-id="68e0e-307">You can use [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) toodo this.</span></span>
>
> ![Linux][Logo_Linux] <span data-ttu-id="68e0e-309">Linux</span><span class="sxs-lookup"><span data-stu-id="68e0e-309">Linux</span></span>
>
> <span data-ttu-id="68e0e-310">tooprepare en Linux-avbildning som du kan använda toodeploy flera virtuella datorer, vissa inställningar för Linux måste vara tas ut eller generaliserad på hello lokala VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-310">tooprepare a Linux image that you can use toodeploy multiple virtual machines, some Linux settings must be abstracted or generalized on hello on-premises VM.</span></span> <span data-ttu-id="68e0e-311">Du kan använda `waagent -deprovision` toodo detta.</span><span class="sxs-lookup"><span data-stu-id="68e0e-311">You can use `waagent -deprovision`  toodo this.</span></span> <span data-ttu-id="68e0e-312">Mer information finns i [avbilda en Linux-dator som körs på Azure] [ virtual-machines-linux-capture-image] och hello [Azure Linux-agenten användarhandboken][virtual-machines-linux-agent-user-guide-command-line-options].</span><span class="sxs-lookup"><span data-stu-id="68e0e-312">For more information, see [Capture a Linux virtual machine running on Azure][virtual-machines-linux-capture-image] and hello [Azure Linux agent user guide][virtual-machines-linux-agent-user-guide-command-line-options].</span></span>
>
>

- - -
<span data-ttu-id="68e0e-313">Du kan förbereda och skapa en anpassad avbildning och sedan använda toocreate flera nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="68e0e-313">You can prepare and create a custom image, and then use it toocreate multiple new VMs.</span></span> <span data-ttu-id="68e0e-314">Detta beskrivs i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-314">This is described in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide].</span></span> <span data-ttu-id="68e0e-315">Konfigurera databasen innehåll genom att använda SAP programvara etablering Manager tooinstall ett nytt SAP-system (återställer en säkerhetskopia av databasen från en virtuell Hårddisk som är bifogade toohello virtuell dator) eller genom att återställa en säkerhetskopia av databasen direkt från Azure storage, om ditt DBMS stöder den.</span><span class="sxs-lookup"><span data-stu-id="68e0e-315">Set up your database content either by using SAP Software Provisioning Manager tooinstall a new SAP system (restores a database backup from a VHD that's attached toohello virtual machine) or by directly restoring a database backup from Azure storage, if your DBMS supports it.</span></span> <span data-ttu-id="68e0e-316">Mer information finns i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide].</span><span class="sxs-lookup"><span data-stu-id="68e0e-316">For more information, see [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide].</span></span> <span data-ttu-id="68e0e-317">Om du redan har installerat en SAP-system på den lokala virtuella datorn (särskilt för tvålagers-system), kan du anpassa hello SAP systeminställningar efter hello distributionen av hello Azure VM hello System Byt namn på sätt som stöds av SAP tillhandahållande av programvara Manager (SAP-kommentar [1619720]).</span><span class="sxs-lookup"><span data-stu-id="68e0e-317">If you have already installed an SAP system on your on-premises VM (especially for two-tier systems), you can adapt hello SAP system settings after hello deployment of hello Azure VM by using hello System Rename procedure supported by SAP Software Provisioning Manager (SAP Note [1619720]).</span></span> <span data-ttu-id="68e0e-318">Annars kan installera du hello SAP-program när du har distribuerat hello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-318">Otherwise, you can install hello SAP software after you deploy hello Azure VM.</span></span>

<span data-ttu-id="68e0e-319">hello visar följande flödesschema hello SAP-specifika sekvens med stegen för att distribuera en virtuell dator från en anpassad avbildning:</span><span class="sxs-lookup"><span data-stu-id="68e0e-319">hello following flowchart shows hello SAP-specific sequence of steps for deploying a VM from a custom image:</span></span>

![Flödesdiagram för distribution av Virtuella datorer för SAP-system med hjälp av en VM-avbildning i privata Marketplace][deployment-guide-figure-300]

#### <a name="create-hello-virtual-machine"></a><span data-ttu-id="68e0e-321">Skapa hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="68e0e-321">Create hello virtual machine</span></span>
<span data-ttu-id="68e0e-322">toocreate en distribution med hjälp av en privat operativsystemsavbildning från hello Azure-portalen med någon av följande SAP mallar hello.</span><span class="sxs-lookup"><span data-stu-id="68e0e-322">toocreate a deployment by using a private OS image from hello Azure portal, use one of hello following SAP templates.</span></span> <span data-ttu-id="68e0e-323">Dessa mallar har publicerats i hello [GitHub-lagringsplatsen för azure-snabbstartsmallar][azure-quickstart-templates-github].</span><span class="sxs-lookup"><span data-stu-id="68e0e-323">These templates are published in hello [azure-quickstart-templates GitHub repository][azure-quickstart-templates-github].</span></span> <span data-ttu-id="68e0e-324">Du kan också manuellt skapa en virtuell dator med hjälp av [PowerShell][virtual-machines-upload-image-windows-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="68e0e-324">You also can manually create a virtual machine, by using [PowerShell][virtual-machines-upload-image-windows-resource-manager].</span></span>

* <span data-ttu-id="68e0e-325">[**Tvålagers-(endast en virtuell dator) konfigurationsmallen** (sap-2-nivå-användaren-bild)][sap-templates-2-tier-user-image]</span><span class="sxs-lookup"><span data-stu-id="68e0e-325">[**Two-tier configuration (only one virtual machine) template** (sap-2-tier-user-image)][sap-templates-2-tier-user-image]</span></span>

  <span data-ttu-id="68e0e-326">toocreate system på två nivåer med bara en virtuell dator kan använda den här mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-326">toocreate a two-tier system by using only one virtual machine, use this template.</span></span>
* <span data-ttu-id="68e0e-327">[**Trelagers-(flera virtuella datorer) konfigurationsmallen** (sap-3-nivå-användaren-bild)][sap-templates-3-tier-user-image]</span><span class="sxs-lookup"><span data-stu-id="68e0e-327">[**Three-tier configuration (multiple virtual machines) template** (sap-3-tier-user-image)][sap-templates-3-tier-user-image]</span></span>

  <span data-ttu-id="68e0e-328">toocreate en trelagers-system med flera virtuella datorer eller egna OS-avbildningen använder den här mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-328">toocreate a three-tier system by using multiple virtual machines or your own OS image, use this template.</span></span>

<span data-ttu-id="68e0e-329">Ange följande parametrar för hello mallen hello i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="68e0e-329">In hello Azure portal, enter hello following parameters for hello template:</span></span>

1. <span data-ttu-id="68e0e-330">**Grunderna**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-330">**Basics**:</span></span>
  * <span data-ttu-id="68e0e-331">**Prenumerationen**: hello prenumeration toouse toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-331">**Subscription**: hello subscription toouse toodeploy hello template.</span></span>
  * <span data-ttu-id="68e0e-332">**Resursgruppen**: hello resource group toouse toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-332">**Resource group**: hello resource group toouse toodeploy hello template.</span></span> <span data-ttu-id="68e0e-333">Du kan skapa en ny resursgrupp eller välj en befintlig resursgrupp i hello prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-333">You can create a new resource group or select an existing resource group in hello subscription.</span></span>
  * <span data-ttu-id="68e0e-334">**Plats**: där toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-334">**Location**: Where toodeploy hello template.</span></span> <span data-ttu-id="68e0e-335">Om du har valt en befintlig resursgrupp används hello plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-335">If you selected an existing resource group, hello location of that resource group is used.</span></span>
2. <span data-ttu-id="68e0e-336">**Inställningar för**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-336">**Settings**:</span></span>
  * <span data-ttu-id="68e0e-337">**System-ID för SAP**: hello SAP System-ID.</span><span class="sxs-lookup"><span data-stu-id="68e0e-337">**SAP System ID**: hello SAP System ID.</span></span>
  * <span data-ttu-id="68e0e-338">**OS-typen**: hello typ av operativsystem du vill använda toodeploy (Windows eller Linux).</span><span class="sxs-lookup"><span data-stu-id="68e0e-338">**OS type**: hello operating system type you want toodeploy (Windows or Linux).</span></span>
  * <span data-ttu-id="68e0e-339">**SAP systemstorlek**: hello storleken på hello SAP-system.</span><span class="sxs-lookup"><span data-stu-id="68e0e-339">**SAP system size**: hello size of hello SAP system.</span></span>

    <span data-ttu-id="68e0e-340">hello antalet SAP hello nya system ger.</span><span class="sxs-lookup"><span data-stu-id="68e0e-340">hello number of SAPS hello new system provides.</span></span> <span data-ttu-id="68e0e-341">Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.</span><span class="sxs-lookup"><span data-stu-id="68e0e-341">If you are not sure how many SAPS hello system requires, ask your SAP Technology Partner or System Integrator.</span></span>
  * <span data-ttu-id="68e0e-342">**Filsystemets tillgänglighet** (endast tre skikt mall): hello filsystemets tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-342">**System availability** (three-tier template only): hello system availability.</span></span>

    <span data-ttu-id="68e0e-343">Välj **HA** för en konfiguration som passar för en installation med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-343">Select **HA** for a configuration that is suitable for a high-availability installation.</span></span> <span data-ttu-id="68e0e-344">Två databasservrar och två servrar för ASCS skapas.</span><span class="sxs-lookup"><span data-stu-id="68e0e-344">Two database servers and two servers for ASCS are created.</span></span>
  * <span data-ttu-id="68e0e-345">**Lagringstyp** (endast två nivåer mall): hello typ av lagring toouse.</span><span class="sxs-lookup"><span data-stu-id="68e0e-345">**Storage type** (two-tier template only): hello type of storage toouse.</span></span>

    <span data-ttu-id="68e0e-346">Vi rekommenderar med Azure Premium-lagring för större system.</span><span class="sxs-lookup"><span data-stu-id="68e0e-346">For larger systems, we highly recommend using Azure Premium Storage.</span></span> <span data-ttu-id="68e0e-347">Mer information om lagringstyper finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="68e0e-347">For more information about storage types, see hello following resources:</span></span>
      * <span data-ttu-id="68e0e-348">[Användning av Azure Premium SSD-lagring för SAP DBMS-instans][2367194]</span><span class="sxs-lookup"><span data-stu-id="68e0e-348">[Use of Azure Premium SSD Storage for SAP DBMS Instance][2367194]</span></span>
      * <span data-ttu-id="68e0e-349">[Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="68e0e-349">[Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
      * <span data-ttu-id="68e0e-350">[Premium-lagring: Högpresterande lagring för arbetsbelastningar på virtuella Azure-datorn][storage-premium-storage-preview-portal]</span><span class="sxs-lookup"><span data-stu-id="68e0e-350">[Premium Storage: High-performance storage for Azure virtual machine workloads][storage-premium-storage-preview-portal]</span></span>
      * <span data-ttu-id="68e0e-351">[Introduktion tooMicrosoft Azure Storage][storage-introduction]</span><span class="sxs-lookup"><span data-stu-id="68e0e-351">[Introduction tooMicrosoft Azure Storage][storage-introduction]</span></span>
  * <span data-ttu-id="68e0e-352">**Användaravbildningen VHD-URI**: hello URI för hello privata OS-avbildningen virtuell Hårddisk, till exempel https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.</span><span class="sxs-lookup"><span data-stu-id="68e0e-352">**User image VHD URI**: hello URI of hello private OS image VHD, for example, https://&lt;accountname>.blob.core.windows.net/vhds/userimage.vhd.</span></span>
  * <span data-ttu-id="68e0e-353">**Användaren avbildningens lagringskonto**: hello namnet på hello lagringskonto där hello privata OS-avbildningen lagras, till exempel &lt;accountname > i https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.</span><span class="sxs-lookup"><span data-stu-id="68e0e-353">**User image storage account**: hello name of hello storage account where hello private OS image is stored, for example, &lt;accountname> in https://&lt;accountname>.blob.core.windows.net/vhds/userimage.vhd.</span></span>
  * <span data-ttu-id="68e0e-354">**Administratörsanvändarnamnet** och **adminlösenord**: hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="68e0e-354">**Admin username** and **Admin password**: hello username and password.</span></span>

    <span data-ttu-id="68e0e-355">En ny användare skapas för att logga in toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-355">A new user is created, for signing in toohello virtual machine.</span></span>
  * <span data-ttu-id="68e0e-356">**Ny eller befintlig undernät**: Anger om ett nytt virtuellt nätverk och undernät har skapats eller ett befintligt undernät används.</span><span class="sxs-lookup"><span data-stu-id="68e0e-356">**New or existing subnet**: Determines whether a new virtual network and subnet is created or an existing subnet is used.</span></span> <span data-ttu-id="68e0e-357">Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-357">If you already have a virtual network that is connected tooyour on-premises network, select **Existing**.</span></span>
  * <span data-ttu-id="68e0e-358">**Undernät-ID**: hello-ID för virtuella datorer i hello undernät toowhich hello ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="68e0e-358">**Subnet ID**: hello ID of hello subnet toowhich hello virtual machines will connect to.</span></span> <span data-ttu-id="68e0e-359">Välj hello undernät för din VPN eller ExpressRoute virtuellt nätverk toouse tooconnect hello tooyour lokalt nätverk för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="68e0e-359">Select hello subnet of your VPN or ExpressRoute virtual network toouse tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="68e0e-360">hello ID ser oftast ut så här:</span><span class="sxs-lookup"><span data-stu-id="68e0e-360">hello ID usually looks like this:</span></span>

    <span data-ttu-id="68e0e-361">/subscriptions/&lt;prenumerations-id > /resourceGroups/&lt;resursgruppens namn > /providers/Microsoft.Network/virtualNetworks/&lt;virtuella nätverksnamnet > /subnets/&lt;undernätsnamn ></span><span class="sxs-lookup"><span data-stu-id="68e0e-361">/subscriptions/&lt;subscription id>/resourceGroups/&lt;resource group name>/providers/Microsoft.Network/virtualNetworks/&lt;virtual network name>/subnets/&lt;subnet name></span></span>

3. <span data-ttu-id="68e0e-362">**Villkor**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-362">**Terms and conditions**:</span></span>  
    <span data-ttu-id="68e0e-363">Granska och Godkänn hello juridiska villkor.</span><span class="sxs-lookup"><span data-stu-id="68e0e-363">Review and accept hello legal terms.</span></span>

4.  <span data-ttu-id="68e0e-364">Välj **inköp**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-364">Select **Purchase**.</span></span>

#### <a name="install-hello-vm-agent-linux-only"></a><span data-ttu-id="68e0e-365">Installera hello VM-agenten (endast Linux)</span><span class="sxs-lookup"><span data-stu-id="68e0e-365">Install hello VM Agent (Linux only)</span></span>
<span data-ttu-id="68e0e-366">toouse hello mallar beskrivs i föregående avsnitt hello hello Linux-agenten måste vara installerad i hello användaravbildningen eller hello distributionen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="68e0e-366">toouse hello templates described in hello preceding section, hello Linux Agent must already be installed in hello user image, or hello deployment will fail.</span></span> <span data-ttu-id="68e0e-367">Hämta och installera hello VM-agenten i hello användaravbildning som beskrivs i [hämta, installera och aktivera hello Azure VM-agenten][deployment-guide-4.4].</span><span class="sxs-lookup"><span data-stu-id="68e0e-367">Download and install hello VM Agent in hello user image as described in [Download, install, and enable hello Azure VM Agent][deployment-guide-4.4].</span></span> <span data-ttu-id="68e0e-368">Om du inte använder hello mallar kan installera du också hello senare VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="68e0e-368">If you don’t use hello templates, you also can install hello VM Agent later.</span></span>

#### <a name="join-a-domain-windows-only"></a><span data-ttu-id="68e0e-369">Anslut till en domän (endast Windows)</span><span class="sxs-lookup"><span data-stu-id="68e0e-369">Join a domain (Windows only)</span></span>
<span data-ttu-id="68e0e-370">Om din Azure-distribution är anslutna tooan lokala Active Directory DNS-instans eller via en Azure-plats-till-plats VPN-anslutning eller Azure ExpressRoute (Detta kallas *mellan lokala* i [Azure virtuella datorer planering och implementering för SAP på Linux][planning-guide]), det förväntas att hello VM är ansluter till en lokal domän.</span><span class="sxs-lookup"><span data-stu-id="68e0e-370">If your Azure deployment is connected tooan on-premises Active Directory or DNS instance via an Azure site-to-site VPN connection or Azure ExpressRoute (this is called *cross-premises* in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), it is expected that hello VM is joining an on-premises domain.</span></span> <span data-ttu-id="68e0e-371">Mer information om överväganden för det här steget finns [ansluta till en VM tooan lokal domän (Windows)][deployment-guide-4.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-371">For more information about considerations for this step, see [Join a VM tooan on-premises domain (Windows only)][deployment-guide-4.3].</span></span>

#### <a name="configure-proxy-settings"></a><span data-ttu-id="68e0e-372">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="68e0e-372">Configure proxy settings</span></span>
<span data-ttu-id="68e0e-373">Beroende på hur det lokala nätverket är konfigurerat kan behöva du tooset in hello proxy på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-373">Depending on how your on-premises network is configured, you might need tooset up hello proxy on your VM.</span></span> <span data-ttu-id="68e0e-374">Om den virtuella datorn är ansluten tooyour lokalt nätverk via VPN eller ExpressRoute, kan hello VM inte vara kan tooaccess hello Internet, och inte kan toodownload hello krävs tillägg eller samla in övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="68e0e-374">If your VM is connected tooyour on-premises network via VPN or ExpressRoute, hello VM might not be able tooaccess hello Internet, and won't be able toodownload hello required extensions or collect monitoring data.</span></span> <span data-ttu-id="68e0e-375">Mer information finns i [konfigurera hello proxy][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="68e0e-375">For more information, see [Configure hello proxy][deployment-guide-configure-proxy].</span></span>

#### <a name="configure-monitoring"></a><span data-ttu-id="68e0e-376">Konfigurera övervakning</span><span class="sxs-lookup"><span data-stu-id="68e0e-376">Configure monitoring</span></span>
<span data-ttu-id="68e0e-377">toobe att din miljö har stöd för SAP, ställa in hello Azure övervakning tillägget för SAP som beskrivs i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="68e0e-377">toobe sure your environment supports SAP, set up hello Azure Monitoring Extension for SAP as described in [Configure hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="68e0e-378">Kontrollera hello krav för SAP övervakning och minsta operativsystemversioner som krävs SAP Kernel och SAP Värdagenten hello resurser som anges i [SAP resurser][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-378">Check hello prerequisites for SAP monitoring, and required minimum versions of SAP Kernel and SAP Host Agent, in hello resources listed in [SAP resources][deployment-guide-2.2].</span></span>

#### <a name="monitoring-check"></a><span data-ttu-id="68e0e-379">Övervakning av kontrollen</span><span class="sxs-lookup"><span data-stu-id="68e0e-379">Monitoring check</span></span>
<span data-ttu-id="68e0e-380">Kontrollera om övervakning fungerar, enligt beskrivningen i [kontroller och felsökning för att ställa in slutpunkt till slutpunkt övervakning][deployment-guide-troubleshooting-chapter].</span><span class="sxs-lookup"><span data-stu-id="68e0e-380">Check whether monitoring is working, as described in [Checks and troubleshooting for setting up end-to-end monitoring][deployment-guide-troubleshooting-chapter].</span></span>




### <span data-ttu-id="68e0e-381"><a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Flytta en virtuell dator lokalt med hjälp av en ej generaliserad Azure VHD med SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-381"><a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Moving an on-premises VM by using a non-generalized Azure VHD with SAP</span></span>
<span data-ttu-id="68e0e-382">I det här scenariot kan du planera toomove ett särskilt SAP-system från en lokal miljö tooAzure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-382">In this scenario, you plan toomove a specific SAP system from an on-premises environment tooAzure.</span></span> <span data-ttu-id="68e0e-383">Du kan göra detta genom att överföra hello VHD som har hello OS hello SAP binärfiler och slutligen hello DBMS binärfiler plus hello virtuella hårddiskar med hello data och loggfiler för hello DBMS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-383">You can do this by uploading hello VHD that has hello OS, hello SAP binaries, and eventually hello DBMS binaries, plus hello VHDs with hello data and log files of hello DBMS, tooAzure.</span></span> <span data-ttu-id="68e0e-384">Till skillnad från hello-scenario som beskrivs i [Scenario 2: distribuera en virtuell dator med en anpassad avbildning för SAP][deployment-guide-3.3], i detta fall kan du behålla hello värdnamn, SAP SID och SAP användarkonton i hello Azure VM, eftersom de var konfigurerad i hello lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="68e0e-384">Unlike hello scenario described in [Scenario 2: Deploying a VM with a custom image for SAP][deployment-guide-3.3], in this case, you keep hello hostname, SAP SID, and SAP user accounts in hello Azure VM, because they were configured in hello on-premises environment.</span></span> <span data-ttu-id="68e0e-385">Du behöver inte toogeneralize hello OS.</span><span class="sxs-lookup"><span data-stu-id="68e0e-385">You do not need toogeneralize hello OS.</span></span> <span data-ttu-id="68e0e-386">Det här scenariot gäller oftast toocross lokala scenarier där en del av hello SAP liggande körs lokalt och en del av den körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-386">This scenario applies most often toocross-premises scenarios where part of hello SAP landscape runs on-premises and part of it runs on Azure.</span></span>

<span data-ttu-id="68e0e-387">I det här scenariot hello VM-agenten installeras inte automatiskt under distributionen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-387">In this scenario, hello VM Agent is not automatically installed during deployment.</span></span> <span data-ttu-id="68e0e-388">Eftersom hello VM-agenten och hello Azure förbättrad övervakning av tillägget för SAP krävs för toorun SAP, måste toodownload, installera och aktivera båda komponenterna manuellt när du skapar hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="68e0e-388">Because hello VM Agent and hello Azure Enhanced Monitoring Extension for SAP required for toorun SAP, you need toodownload, install, and enable both components manually after you create hello virtual machine.</span></span>

<span data-ttu-id="68e0e-389">Mer information om hello Azure VM-agenten finns i hello följande resurser.</span><span class="sxs-lookup"><span data-stu-id="68e0e-389">For more information about hello Azure VM Agent, see hello following resources.</span></span>

[comment]: <> (MSSedusch TODO uppdatera Windows länk nedan)

- - -
> ![Windows][Logo_Windows] <span data-ttu-id="68e0e-392">Windows</span><span class="sxs-lookup"><span data-stu-id="68e0e-392">Windows</span></span>
>
> <span data-ttu-id="68e0e-393"><http://blogs.msdn.com/b/wats/Archive/2014/02/17/BgInfo-guest-Agent-Extension-for-Azure-VMS.aspx></span><span class="sxs-lookup"><span data-stu-id="68e0e-393"><http://blogs.msdn.com/b/wats/archive/2014/02/17/bginfo-guest-agent-extension-for-azure-vms.aspx></span></span>
>
> ![Linux][Logo_Linux] <span data-ttu-id="68e0e-395">Linux</span><span class="sxs-lookup"><span data-stu-id="68e0e-395">Linux</span></span>
>
> <span data-ttu-id="68e0e-396">[Användarhandboken för Azure Linux-Agent][virtual-machines-linux-agent-user-guide]</span><span class="sxs-lookup"><span data-stu-id="68e0e-396">[Azure Linux Agent User Guide][virtual-machines-linux-agent-user-guide]</span></span>
>
>

- - -

<span data-ttu-id="68e0e-397">hello följande flödesschema visar hello sekvens med steg för att flytta en virtuell dator lokalt med hjälp av en ej generaliserad Azure VHD:</span><span class="sxs-lookup"><span data-stu-id="68e0e-397">hello following flowchart shows hello sequence of steps for moving an on-premises VM by using a non-generalized Azure VHD:</span></span>

![Flödesdiagram för distribution av Virtuella datorer för SAP-system med hjälp av en virtuell disk][deployment-guide-figure-400]

<span data-ttu-id="68e0e-399">Förutsatt att hello disken är redan överförts och definieras i Azure (se [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide]), gör hello uppgifter beskrivs i hello bredvid avsnitten.</span><span class="sxs-lookup"><span data-stu-id="68e0e-399">Assuming that hello disk is already uploaded and defined in Azure (see [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), do hello tasks described in hello next few sections.</span></span>

#### <a name="create-a-virtual-machine"></a><span data-ttu-id="68e0e-400">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="68e0e-400">Create a virtual machine</span></span>
<span data-ttu-id="68e0e-401">toocreate en distribution med hjälp av en privat OS-disk via hello Azure-portalen använder hello SAP mallen publicerats i hello [GitHub-lagringsplatsen för azure-snabbstartsmallar][azure-quickstart-templates-github].</span><span class="sxs-lookup"><span data-stu-id="68e0e-401">toocreate a deployment by using a private OS disk through hello Azure portal, use hello SAP template published in hello [azure-quickstart-templates GitHub repository][azure-quickstart-templates-github].</span></span> <span data-ttu-id="68e0e-402">Du kan också manuellt skapa en virtuell dator med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="68e0e-402">You also can manually create a virtual machine, by using PowerShell.</span></span>

* <span data-ttu-id="68e0e-403">[**Tvålagers-(endast en virtuell dator) konfigurationsmallen** (sap-2-nivå-användaren-disk)][sap-templates-2-tier-os-disk]</span><span class="sxs-lookup"><span data-stu-id="68e0e-403">[**Two-tier configuration (only one virtual machine) template** (sap-2-tier-user-disk)][sap-templates-2-tier-os-disk]</span></span>

  <span data-ttu-id="68e0e-404">toocreate system på två nivåer med bara en virtuell dator kan använda den här mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-404">toocreate a two-tier system by using only one virtual machine, use this template.</span></span>

<span data-ttu-id="68e0e-405">Ange följande parametrar för hello mallen hello i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="68e0e-405">In hello Azure portal, enter hello following parameters for hello template:</span></span>

1. <span data-ttu-id="68e0e-406">**Grunderna**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-406">**Basics**:</span></span>
  * <span data-ttu-id="68e0e-407">**Prenumerationen**: hello prenumeration toouse toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-407">**Subscription**: hello subscription toouse toodeploy hello template.</span></span>
  * <span data-ttu-id="68e0e-408">**Resursgruppen**: hello resource group toouse toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-408">**Resource group**: hello resource group toouse toodeploy hello template.</span></span> <span data-ttu-id="68e0e-409">Du kan skapa en ny resursgrupp eller välj en befintlig resursgrupp i hello prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-409">You can create a new resource group or select an existing resource group in hello subscription.</span></span>
  * <span data-ttu-id="68e0e-410">**Plats**: där toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-410">**Location**: Where toodeploy hello template.</span></span> <span data-ttu-id="68e0e-411">Om du har valt en befintlig resursgrupp används hello plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-411">If you selected an existing resource group, hello location of that resource group is used.</span></span>
2. <span data-ttu-id="68e0e-412">**Inställningar för**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-412">**Settings**:</span></span>
  * <span data-ttu-id="68e0e-413">**System-ID för SAP**: hello SAP System-ID.</span><span class="sxs-lookup"><span data-stu-id="68e0e-413">**SAP System ID**: hello SAP System ID.</span></span>
  * <span data-ttu-id="68e0e-414">**OS-typen**: hello typ av operativsystem du vill använda toodeploy (Windows eller Linux).</span><span class="sxs-lookup"><span data-stu-id="68e0e-414">**OS type**: hello operating system type you want toodeploy (Windows or Linux).</span></span>
  * <span data-ttu-id="68e0e-415">**SAP systemstorlek**: hello storleken på hello SAP-system.</span><span class="sxs-lookup"><span data-stu-id="68e0e-415">**SAP system size**: hello size of hello SAP system.</span></span>

    <span data-ttu-id="68e0e-416">hello antalet SAP hello nya system ger.</span><span class="sxs-lookup"><span data-stu-id="68e0e-416">hello number of SAPS hello new system provides.</span></span> <span data-ttu-id="68e0e-417">Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.</span><span class="sxs-lookup"><span data-stu-id="68e0e-417">If you are not sure how many SAPS hello system requires, ask your SAP Technology Partner or System Integrator.</span></span>
  * <span data-ttu-id="68e0e-418">**Lagringstyp** (endast två nivåer mall): hello typ av lagring toouse.</span><span class="sxs-lookup"><span data-stu-id="68e0e-418">**Storage type** (two-tier template only): hello type of storage toouse.</span></span>

    <span data-ttu-id="68e0e-419">Vi rekommenderar med Azure Premium-lagring för större system.</span><span class="sxs-lookup"><span data-stu-id="68e0e-419">For larger systems, we highly recommend using Azure Premium Storage.</span></span> <span data-ttu-id="68e0e-420">Mer information om lagringstyper finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="68e0e-420">For more information about storage types, see hello following resources:</span></span>
      * <span data-ttu-id="68e0e-421">[Användning av Azure Premium SSD-lagring för SAP DBMS-instans][2367194]</span><span class="sxs-lookup"><span data-stu-id="68e0e-421">[Use of Azure Premium SSD Storage for SAP DBMS Instance][2367194]</span></span>
      * <span data-ttu-id="68e0e-422">[Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella DBMS-distribution för SAP på Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="68e0e-422">[Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machine DBMS deployment for SAP on Linux][dbms-guide]</span></span>
      * <span data-ttu-id="68e0e-423">[Premium-lagring: Högpresterande lagring för arbetsbelastningar på Azure-dator][storage-premium-storage-preview-portal]</span><span class="sxs-lookup"><span data-stu-id="68e0e-423">[Premium Storage: High-performance storage for Azure Virtual Machine workloads][storage-premium-storage-preview-portal]</span></span>
      * <span data-ttu-id="68e0e-424">[Introduktion tooMicrosoft Azure Storage][storage-introduction]</span><span class="sxs-lookup"><span data-stu-id="68e0e-424">[Introduction tooMicrosoft Azure Storage][storage-introduction]</span></span>
  * <span data-ttu-id="68e0e-425">**OS-disken VHD-URI**: hello URI för hello privata OS-disk, till exempel https://&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.</span><span class="sxs-lookup"><span data-stu-id="68e0e-425">**OS disk VHD URI**: hello URI of hello private OS disk, for example, https://&lt;accountname>.blob.core.windows.net/vhds/osdisk.vhd.</span></span>
  * <span data-ttu-id="68e0e-426">**Ny eller befintlig undernät**: Anger om skapas ett nytt virtuellt nätverk och undernät eller ett befintligt undernät används.</span><span class="sxs-lookup"><span data-stu-id="68e0e-426">**New or existing subnet**: Determines whether a new virtual network and subnet are created, or an existing subnet is used.</span></span> <span data-ttu-id="68e0e-427">Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-427">If you already have a virtual network that is connected tooyour on-premises network, select **Existing**.</span></span>
  * <span data-ttu-id="68e0e-428">**Undernät-ID**: hello-ID för virtuella datorer i hello undernät toowhich hello ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="68e0e-428">**Subnet ID**: hello ID of hello subnet toowhich hello virtual machines will connect to.</span></span> <span data-ttu-id="68e0e-429">Välj hello undernät för din VPN- eller Azure ExpressRoute virtuellt nätverk toouse tooconnect hello tooyour lokalt nätverk för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="68e0e-429">Select hello subnet of your VPN or Azure ExpressRoute virtual network toouse tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="68e0e-430">hello ID ser oftast ut så här:</span><span class="sxs-lookup"><span data-stu-id="68e0e-430">hello ID usually looks like this:</span></span>

    <span data-ttu-id="68e0e-431">/subscriptions/&lt;prenumerations-id > /resourceGroups/&lt;resursgruppens namn > /providers/Microsoft.Network/virtualNetworks/&lt;virtuella nätverksnamnet > /subnets/&lt;undernätsnamn ></span><span class="sxs-lookup"><span data-stu-id="68e0e-431">/subscriptions/&lt;subscription id>/resourceGroups/&lt;resource group name>/providers/Microsoft.Network/virtualNetworks/&lt;virtual network name>/subnets/&lt;subnet name></span></span>

3. <span data-ttu-id="68e0e-432">**Villkor**:</span><span class="sxs-lookup"><span data-stu-id="68e0e-432">**Terms and conditions**:</span></span>  
    <span data-ttu-id="68e0e-433">Granska och Godkänn hello juridiska villkor.</span><span class="sxs-lookup"><span data-stu-id="68e0e-433">Review and accept hello legal terms.</span></span>

4.  <span data-ttu-id="68e0e-434">Välj **inköp**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-434">Select **Purchase**.</span></span>

#### <a name="install-hello-vm-agent"></a><span data-ttu-id="68e0e-435">Installera hello VM-Agent</span><span class="sxs-lookup"><span data-stu-id="68e0e-435">Install hello VM Agent</span></span>
<span data-ttu-id="68e0e-436">toouse hello mallar beskrivs i hello föregående avsnitt, hello VM-agenten måste installeras på hello OS-disk eller hello distributionen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="68e0e-436">toouse hello templates described in hello preceding section, hello VM Agent must be installed on hello OS disk, or hello deployment will fail.</span></span> <span data-ttu-id="68e0e-437">Hämta och installera hello VM-agenten i hello VM, enligt beskrivningen i [hämta, installera och aktivera hello Azure VM-agenten][deployment-guide-4.4].</span><span class="sxs-lookup"><span data-stu-id="68e0e-437">Download and install hello VM Agent in hello VM, as described in [Download, install, and enable hello Azure VM Agent][deployment-guide-4.4].</span></span>

<span data-ttu-id="68e0e-438">Om du inte använder hello-mallar som beskrivs i föregående avsnitt hello, kan du även installera hello VM-agenten efteråt.</span><span class="sxs-lookup"><span data-stu-id="68e0e-438">If you don’t use hello templates described in hello preceding section, you can also install hello VM Agent afterwards.</span></span>

#### <a name="join-a-domain-windows-only"></a><span data-ttu-id="68e0e-439">Anslut till en domän (endast Windows)</span><span class="sxs-lookup"><span data-stu-id="68e0e-439">Join a domain (Windows only)</span></span>
<span data-ttu-id="68e0e-440">Om din Azure-distribution är anslutna tooan lokala Active Directory DNS-instans eller via en Azure-plats-till-plats VPN-anslutning eller ExpressRoute (Detta kallas *mellan lokala* i [planering av virtuella datorer i Azure och implementeringen för SAP på Linux][planning-guide]), det förväntas att hello VM är ansluter till en lokal domän.</span><span class="sxs-lookup"><span data-stu-id="68e0e-440">If your Azure deployment is connected tooan on-premises Active Directory or DNS instance via an Azure site-to-site VPN connection or ExpressRoute (this is called *cross-premises* in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), it is expected that hello VM is joining an on-premises domain.</span></span> <span data-ttu-id="68e0e-441">Mer information om överväganden för den här aktiviteten finns [ansluta till en VM tooan lokal domän (Windows)][deployment-guide-4.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-441">For more information about considerations for this task, see [Join a VM tooan on-premises domain (Windows only)][deployment-guide-4.3].</span></span>

#### <a name="configure-proxy-settings"></a><span data-ttu-id="68e0e-442">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="68e0e-442">Configure proxy settings</span></span>
<span data-ttu-id="68e0e-443">Beroende på hur det lokala nätverket är konfigurerat kan behöva du tooset in hello proxy på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-443">Depending on how your on-premises network is configured, you might need tooset up hello proxy on your VM.</span></span> <span data-ttu-id="68e0e-444">Om den virtuella datorn är ansluten tooyour lokalt nätverk via VPN eller ExpressRoute, kan hello VM inte vara kan tooaccess hello Internet, och inte kan toodownload hello krävs tillägg eller samla in övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="68e0e-444">If your VM is connected tooyour on-premises network via VPN or ExpressRoute, hello VM might not be able tooaccess hello Internet, and won't be able toodownload hello required extensions or collect monitoring data.</span></span> <span data-ttu-id="68e0e-445">Mer information finns i [konfigurera hello proxy][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="68e0e-445">For more information, see [Configure hello proxy][deployment-guide-configure-proxy].</span></span>

#### <a name="configure-monitoring"></a><span data-ttu-id="68e0e-446">Konfigurera övervakning</span><span class="sxs-lookup"><span data-stu-id="68e0e-446">Configure monitoring</span></span>
<span data-ttu-id="68e0e-447">toobe att din miljö har stöd för SAP, ställa in hello Azure övervakning tillägget för SAP som beskrivs i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="68e0e-447">toobe sure your environment supports SAP, set up hello Azure Monitoring Extension for SAP as described in [Configure hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="68e0e-448">Kontrollera hello krav för SAP övervakning och minsta operativsystemversioner som krävs SAP Kernel och SAP Värdagenten hello resurser som anges i [SAP resurser][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-448">Check hello prerequisites for SAP monitoring, and required minimum versions of SAP Kernel and SAP Host Agent, in hello resources listed in [SAP resources][deployment-guide-2.2].</span></span>

#### <a name="monitoring-check"></a><span data-ttu-id="68e0e-449">Övervakning av kontrollen</span><span class="sxs-lookup"><span data-stu-id="68e0e-449">Monitoring check</span></span>
<span data-ttu-id="68e0e-450">Kontrollera om övervakning fungerar, enligt beskrivningen i [kontroller och felsökning för att ställa in slutpunkt till slutpunkt övervakning][deployment-guide-troubleshooting-chapter].</span><span class="sxs-lookup"><span data-stu-id="68e0e-450">Check whether monitoring is working, as described in [Checks and troubleshooting for setting up end-to-end monitoring][deployment-guide-troubleshooting-chapter].</span></span>

## <a name="update-hello-monitoring-configuration-for-sap"></a><span data-ttu-id="68e0e-451">Uppdatera hello övervakningskonfigurationen för SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-451">Update hello monitoring configuration for SAP</span></span>
<span data-ttu-id="68e0e-452">Uppdatera hello SAP övervakningskonfigurationen i någon av hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="68e0e-452">Update hello SAP monitoring configuration in any of hello following scenarios:</span></span>
* <span data-ttu-id="68e0e-453">hello gemensamma Microsoft/SAP-teamet utökar hello övervakningsfunktionerna och begär fler eller färre räknare.</span><span class="sxs-lookup"><span data-stu-id="68e0e-453">hello joint Microsoft/SAP team extends hello monitoring capabilities and requests more or fewer counters.</span></span>
* <span data-ttu-id="68e0e-454">Microsoft introducerar en ny version av hello underliggande Azure-infrastrukturen som levererar hello övervakningsdata och hello Azure förbättrad övervakning av tillägget för SAP behov toobe anpassas toothose ändringar.</span><span class="sxs-lookup"><span data-stu-id="68e0e-454">Microsoft introduces a new version of hello underlying Azure infrastructure that delivers hello monitoring data, and hello Azure Enhanced Monitoring Extension for SAP needs toobe adapted toothose changes.</span></span>
* <span data-ttu-id="68e0e-455">Du monterar ytterligare virtuella hårddiskar tooyour Azure VM eller du tar bort en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="68e0e-455">You mount additional VHDs tooyour Azure VM or you remove a VHD.</span></span> <span data-ttu-id="68e0e-456">Uppdatera hello samling lagringsrelaterade data i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="68e0e-456">In this scenario, update hello collection of storage-related data.</span></span> <span data-ttu-id="68e0e-457">Ändra din konfiguration genom att lägga till eller ta bort slutpunkter eller genom att tilldela IP-påverkar adresser tooa VM inte hello övervakningskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-457">Changing your configuration by adding or deleting endpoints or by assigning IP addresses tooa VM does not affect hello monitoring configuration.</span></span>
* <span data-ttu-id="68e0e-458">Du ändrar hello storleken på den virtuella datorn i Azure, till exempel från storlek A5 tooany andra VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="68e0e-458">You change hello size of your Azure VM, for example, from size A5 tooany other VM size.</span></span>
* <span data-ttu-id="68e0e-459">Du lägger till nya nätverk gränssnitt tooyour Azure VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-459">You add new network interfaces tooyour Azure VM.</span></span>

<span data-ttu-id="68e0e-460">tooupdate övervakning, uppdatering hello övervakning infrastruktur genom att följa hello stegen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="68e0e-460">tooupdate monitoring settings, update hello monitoring infrastructure by following hello steps in [Configure hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span>

## <a name="detailed-tasks-for-sap-software-deployment-on-a-windows-vm"></a><span data-ttu-id="68e0e-461">Detaljerad information om SAP-programdistribution på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="68e0e-461">Detailed tasks for SAP software deployment on a Windows VM</span></span>
<span data-ttu-id="68e0e-462">Det här avsnittet innehåller detaljerad steg för specifika uppgifter pågående hello konfiguration och distribution.</span><span class="sxs-lookup"><span data-stu-id="68e0e-462">This section has detailed steps for doing specific tasks in hello configuration and deployment process.</span></span>

### <span data-ttu-id="68e0e-463"><a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Distribuera Azure PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="68e0e-463"><a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Deploy Azure PowerShell cmdlets</span></span>
1.  <span data-ttu-id="68e0e-464">Gå för[hämtningsbara filer från Microsoft Azure](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="68e0e-464">Go too[Microsoft Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>
2.  <span data-ttu-id="68e0e-465">Under **kommandoradsverktyg**under **PowerShell**väljer **Windows installera**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-465">Under **Command-line tools**, under **PowerShell**, select **Windows install**.</span></span>
3.  <span data-ttu-id="68e0e-466">Hello Microsoft Download Manager i dialogrutan för hello ned filen (till exempel WindowsAzurePowershellGet.3f.3f.3fnew.exe), Välj **kör**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-466">In hello Microsoft Download Manager dialog box, for hello downloaded file (for example, WindowsAzurePowershellGet.3f.3f.3fnew.exe), select **Run**.</span></span>
4.  <span data-ttu-id="68e0e-467">Välj toorun Microsoft Web Platform Installer (Microsoft Web PI) **Ja**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-467">toorun Microsoft Web Platform Installer (Microsoft Web PI), select **Yes**.</span></span>
5.  <span data-ttu-id="68e0e-468">En sida som ser ut som om det här visas:</span><span class="sxs-lookup"><span data-stu-id="68e0e-468">A page that looks like this appears:</span></span>

  <span data-ttu-id="68e0e-469">![Installationssidan för Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a></span><span class="sxs-lookup"><span data-stu-id="68e0e-469">![Installation page for Azure PowerShell cmdlets][deployment-guide-figure-500]<a name="figure-5"></a></span></span>

6.  <span data-ttu-id="68e0e-470">Välj **installera**, och acceptera licensvillkoren för hello.</span><span class="sxs-lookup"><span data-stu-id="68e0e-470">Select **Install**, and then accept hello Microsoft Software License Terms.</span></span>
7.  <span data-ttu-id="68e0e-471">PowerShell har installerats.</span><span class="sxs-lookup"><span data-stu-id="68e0e-471">PowerShell is installed.</span></span> <span data-ttu-id="68e0e-472">Välj **Slutför** tooclose hello-installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="68e0e-472">Select **Finish** tooclose hello installation wizard.</span></span>

<span data-ttu-id="68e0e-473">Kontrollera ofta för uppdateringar toohello PowerShell cmdlet: ar, vilket vanligtvis uppdateras varje månad.</span><span class="sxs-lookup"><span data-stu-id="68e0e-473">Check frequently for updates toohello PowerShell cmdlets, which usually are updated monthly.</span></span> <span data-ttu-id="68e0e-474">hello enklaste sättet toocheck för uppdateringar är toodo hello föregående installationssteg upp toohello installation sida som visas i steg 5.</span><span class="sxs-lookup"><span data-stu-id="68e0e-474">hello easiest way toocheck for updates is toodo hello preceding installation steps, up toohello installation page shown in step 5.</span></span> <span data-ttu-id="68e0e-475">hello datum och version versionsnumret hello cmdlets finns på hello-sidan som visas i steg 5.</span><span class="sxs-lookup"><span data-stu-id="68e0e-475">hello release date and release number of hello cmdlets are included on hello page shown in step 5.</span></span> <span data-ttu-id="68e0e-476">Om inget annat anges i SAP-kommentar [1928533] eller SAP-kommentar [2015553], rekommenderar vi att du arbetar med hello senaste versionen av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="68e0e-476">Unless stated otherwise in SAP Note [1928533] or SAP Note [2015553], we recommend that you work with hello latest version of Azure PowerShell cmdlets.</span></span>

<span data-ttu-id="68e0e-477">toocheck hello version av hello Azure PowerShell-cmdlets som är installerade på datorn, Kör PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="68e0e-477">toocheck hello version of hello Azure PowerShell cmdlets that are installed on your computer, run this PowerShell command:</span></span>
```powershell
Import-Module Azure
(Get-Module Azure).Version
```
<span data-ttu-id="68e0e-478">hello resultatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="68e0e-478">hello result looks like this:</span></span>

<span data-ttu-id="68e0e-479">![Resultatet av kontroll av Azure PowerShell-cmdlet-version][deployment-guide-figure-600]
<a name="figure-6"></a></span><span class="sxs-lookup"><span data-stu-id="68e0e-479">![Result of Azure PowerShell cmdlet version check][deployment-guide-figure-600]
<a name="figure-6"></a></span></span>

<span data-ttu-id="68e0e-480">Om hello Azure cmdlet version är installerad på datorn är hello aktuell version hello första sidan i guiden för installation av hello anger den genom att lägga till **(installerad)** toohello Produkttitel (se följande skärmbild hello).</span><span class="sxs-lookup"><span data-stu-id="68e0e-480">If hello Azure cmdlet version installed on your computer is hello current version, hello first page of hello installation wizard indicates it by adding **(Installed)** toohello product title (see hello following screenshot).</span></span> <span data-ttu-id="68e0e-481">PowerShell Azure-cmdlets är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="68e0e-481">Your PowerShell Azure cmdlets are up-to-date.</span></span> <span data-ttu-id="68e0e-482">tooclose hello installationsguiden väljer **avsluta**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-482">tooclose hello installation wizard, select **Exit**.</span></span>

<span data-ttu-id="68e0e-483">![Installationssidan för Azure PowerShell-cmdlets som anger att hello senaste versionen av Azure PowerShell-cmdlets är installerade][deployment-guide-figure-700]
<a name="figure-7"></a></span><span class="sxs-lookup"><span data-stu-id="68e0e-483">![Installation page for Azure PowerShell cmdlets indicating that hello most recent version of Azure PowerShell cmdlets are installed][deployment-guide-figure-700]
<a name="figure-7"></a></span></span>

### <span data-ttu-id="68e0e-484"><a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Distribuera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="68e0e-484"><a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Deploy Azure CLI</span></span>
1.  <span data-ttu-id="68e0e-485">Gå för[hämtningsbara filer från Microsoft Azure](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="68e0e-485">Go too[Microsoft Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>
2.  <span data-ttu-id="68e0e-486">Under **kommandoradsverktyg**under **Azure-kommandoradsgränssnittet**väljer hello **installera** länk för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="68e0e-486">Under **Command-line tools**, under **Azure command-line interface**, select hello **Install** link for your operating system.</span></span>
3.  <span data-ttu-id="68e0e-487">Hello Microsoft Download Manager i dialogrutan för hello ned filen (till exempel WindowsAzureXPlatCLI.3f.3f.3fnew.exe), Välj **kör**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-487">In hello Microsoft Download Manager dialog box, for hello downloaded file (for example, WindowsAzureXPlatCLI.3f.3f.3fnew.exe), select **Run**.</span></span>
4.  <span data-ttu-id="68e0e-488">Välj toorun Microsoft Web Platform Installer (Microsoft Web PI) **Ja**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-488">toorun Microsoft Web Platform Installer (Microsoft Web PI), select **Yes**.</span></span>
5.  <span data-ttu-id="68e0e-489">En sida som ser ut som om det här visas:</span><span class="sxs-lookup"><span data-stu-id="68e0e-489">A page that looks like this appears:</span></span>

  <span data-ttu-id="68e0e-490">![Installationssidan för Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a></span><span class="sxs-lookup"><span data-stu-id="68e0e-490">![Installation page for Azure PowerShell cmdlets][deployment-guide-figure-500]<a name="figure-5"></a></span></span>

6.  <span data-ttu-id="68e0e-491">Välj **installera**, och acceptera licensvillkoren för hello.</span><span class="sxs-lookup"><span data-stu-id="68e0e-491">Select **Install**, and then accept hello Microsoft Software License Terms.</span></span>
7.  <span data-ttu-id="68e0e-492">Azure CLI är installerad.</span><span class="sxs-lookup"><span data-stu-id="68e0e-492">Azure CLI is installed.</span></span> <span data-ttu-id="68e0e-493">Välj **Slutför** tooclose hello-installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="68e0e-493">Select **Finish** tooclose hello installation wizard.</span></span>

<span data-ttu-id="68e0e-494">Kontrollera ofta för uppdateringar tooAzure CLI, som vanligtvis uppdateras varje månad.</span><span class="sxs-lookup"><span data-stu-id="68e0e-494">Check frequently for updates tooAzure CLI, which usually is updated monthly.</span></span> <span data-ttu-id="68e0e-495">hello enklaste sättet toocheck för uppdateringar är toodo hello föregående installationssteg upp toohello installation sida som visas i steg 5.</span><span class="sxs-lookup"><span data-stu-id="68e0e-495">hello easiest way toocheck for updates is toodo hello preceding installation steps, up toohello installation page shown in step 5.</span></span>


<span data-ttu-id="68e0e-496">toocheck hello versionen av Azure CLI som är installerad på datorn som kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="68e0e-496">toocheck hello version of Azure CLI that is installed on your computer, run this command:</span></span>
```
azure --version
```

<span data-ttu-id="68e0e-497">hello resultatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="68e0e-497">hello result looks like this:</span></span>

<span data-ttu-id="68e0e-498">![Resultatet av kontroll av Azure CLI version][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a></span><span class="sxs-lookup"><span data-stu-id="68e0e-498">![Result of Azure CLI version check][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a></span></span>

### <span data-ttu-id="68e0e-499"><a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Anslut till en VM tooan lokal domän (endast Windows)</span><span class="sxs-lookup"><span data-stu-id="68e0e-499"><a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Join a VM tooan on-premises domain (Windows only)</span></span>
<span data-ttu-id="68e0e-500">Om du distribuerar SAP virtuella datorer i scenarion med flera lokaler förväntas där lokala Active Directory och DNS har utökats i Azure, att hello virtuella datorer ansluter till en lokal domän.</span><span class="sxs-lookup"><span data-stu-id="68e0e-500">If you deploy SAP VMs in a cross-premises scenario, where on-premises Active Directory and DNS are extended in Azure, it is expected that hello VMs are joining an on-premises domain.</span></span> <span data-ttu-id="68e0e-501">Hej detaljerade steg som du vidta toojoin en VM tooan lokal domän och hello toobe ytterligare programvara som krävs för en medlem av en lokal domän, varierar efter kund.</span><span class="sxs-lookup"><span data-stu-id="68e0e-501">hello detailed steps you take toojoin a VM tooan on-premises domain, and hello additional software required toobe a member of an on-premises domain, varies by customer.</span></span> <span data-ttu-id="68e0e-502">Vanligtvis toojoin en VM-tooan lokala domän måste du tooinstall ytterligare programvara, t.ex. program mot skadlig kod och programvara för säkerhetskopiering och övervakning.</span><span class="sxs-lookup"><span data-stu-id="68e0e-502">Usually, toojoin a VM tooan on-premises domain, you need tooinstall additional software, like antimalware software, and backup or monitoring software.</span></span>

<span data-ttu-id="68e0e-503">I det här scenariot måste du också toomake till att om Internet-proxyinställningarna tvingas när en virtuell dator ansluter till en domän i din miljö, hello Windows lokala System-kontot (S-1-5-18) i hello gäst VM har hello samma proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="68e0e-503">In this scenario, you also need toomake sure that if Internet proxy settings are forced when a VM joins a domain in your environment, hello Windows Local System Account (S-1-5-18) in hello Guest VM has hello same proxy settings.</span></span> <span data-ttu-id="68e0e-504">hello enklaste alternativet är tooforce hello proxy med hjälp av en grupprincip som gäller toosystems i hello domän-domän.</span><span class="sxs-lookup"><span data-stu-id="68e0e-504">hello easiest option is tooforce hello proxy by using a domain Group Policy, which applies toosystems in hello domain.</span></span>

### <span data-ttu-id="68e0e-505"><a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Hämta, installera och aktivera hello Azure VM-agenten</span><span class="sxs-lookup"><span data-stu-id="68e0e-505"><a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Download, install, and enable hello Azure VM Agent</span></span>
<span data-ttu-id="68e0e-506">För virtuella datorer som distribueras från en operativsystemavbildning som inte är generaliserad (till exempel en avbildning som inte kommer från hello Windows-systemförberedelse eller sysprep-verktyget), behöver du toomanually hämta, installera och aktivera hello Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="68e0e-506">For virtual machines that are deployed from an OS image that is not generalized (for example, an image that doesn't originate in hello Windows System Preparation, or sysprep, tool), you need toomanually download, install, and enable hello Azure VM Agent.</span></span>

<span data-ttu-id="68e0e-507">Det här steget är inte nödvändigt om du distribuerar en virtuell dator från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="68e0e-507">If you deploy a VM from hello Azure Marketplace, this step is not required.</span></span> <span data-ttu-id="68e0e-508">Bilder från hello Azure Marketplace redan hello Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="68e0e-508">Images from hello Azure Marketplace already have hello Azure VM Agent.</span></span>

#### <span data-ttu-id="68e0e-509"><a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows</span><span class="sxs-lookup"><span data-stu-id="68e0e-509"><a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows</span></span>
1.  <span data-ttu-id="68e0e-510">Hämta hello Azure VM-agenten:</span><span class="sxs-lookup"><span data-stu-id="68e0e-510">Download hello Azure VM Agent:</span></span>
  1.  <span data-ttu-id="68e0e-511">Hämta hello [Azure VM-agenten installationspaketet](https://go.microsoft.com/fwlink/?LinkId=394789).</span><span class="sxs-lookup"><span data-stu-id="68e0e-511">Download hello [Azure VM Agent installer package](https://go.microsoft.com/fwlink/?LinkId=394789).</span></span>
  2.  <span data-ttu-id="68e0e-512">Lagra hello VM-agenten MSI-paketet lokalt på en dator eller server.</span><span class="sxs-lookup"><span data-stu-id="68e0e-512">Store hello VM Agent MSI package locally on a personal computer or server.</span></span>
2.  <span data-ttu-id="68e0e-513">Installera hello Azure VM-agenten:</span><span class="sxs-lookup"><span data-stu-id="68e0e-513">Install hello Azure VM Agent:</span></span>
  1.  <span data-ttu-id="68e0e-514">Ansluta toohello distribueras virtuella Azure-datorn med hjälp av Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="68e0e-514">Connect toohello deployed Azure VM by using Remote Desktop Protocol (RDP).</span></span>
  2.  <span data-ttu-id="68e0e-515">Öppna Utforskaren på hello VM och välj hello målkatalogen för hello MSI-filen för hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="68e0e-515">Open a Windows Explorer window on hello VM and select hello target directory for hello MSI file of hello VM Agent.</span></span>
  3.  <span data-ttu-id="68e0e-516">Dra hello Azure VM-agenten Installer MSI-fil från din lokala dator-servern toohello målkatalogen för hello VM-agenten på hello VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-516">Drag hello Azure VM Agent Installer MSI file from your local computer/server toohello target directory of hello VM Agent on hello VM.</span></span>
  4.  <span data-ttu-id="68e0e-517">Dubbelklicka på hello MSI-filen på hello VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-517">Double-click hello MSI file on hello VM.</span></span>
3.  <span data-ttu-id="68e0e-518">För virtuella datorer som är kopplade tooon lokala domäner, kontrollera att eventuell Internet proxy-inställningar också gäller toohello Windows lokala systemkontot (S-1-5-18) hello VM, enligt beskrivningen i [konfigurera hello proxy] [ deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="68e0e-518">For VMs that are joined tooon-premises domains, make sure that eventual Internet proxy settings also apply toohello Windows Local System account (S-1-5-18) in hello VM, as described in [Configure hello proxy][deployment-guide-configure-proxy].</span></span> <span data-ttu-id="68e0e-519">hello VM-agenten körs i den här kontexten och måste toobe kan tooconnect tooAzure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-519">hello VM Agent runs in this context and needs toobe able tooconnect tooAzure.</span></span>

<span data-ttu-id="68e0e-520">Inga användaråtgärder är obligatoriska tooupdate hello Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="68e0e-520">No user interaction is required tooupdate hello Azure VM Agent.</span></span> <span data-ttu-id="68e0e-521">hello VM-agenten uppdateras automatiskt och kräver inte en VM-omstart.</span><span class="sxs-lookup"><span data-stu-id="68e0e-521">hello VM Agent is automatically updated, and does not require a VM restart.</span></span>

#### <span data-ttu-id="68e0e-522"><a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux</span><span class="sxs-lookup"><span data-stu-id="68e0e-522"><a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux</span></span>
<span data-ttu-id="68e0e-523">Använd följande kommandon tooinstall hello VM-Agent för Linux hello:</span><span class="sxs-lookup"><span data-stu-id="68e0e-523">Use hello following commands tooinstall hello VM Agent for Linux:</span></span>

* <span data-ttu-id="68e0e-524">**SUSE Linux Enterprise Server (SLES)**</span><span class="sxs-lookup"><span data-stu-id="68e0e-524">**SUSE Linux Enterprise Server (SLES)**</span></span>

  ```
  sudo zypper install WALinuxAgent
  ```

* <span data-ttu-id="68e0e-525">**Red Hat Enterprise Linux (RHEL)**</span><span class="sxs-lookup"><span data-stu-id="68e0e-525">**Red Hat Enterprise Linux (RHEL)**</span></span>

  ```
  sudo yum install WALinuxAgent
  ```

<span data-ttu-id="68e0e-526">Om hello agenten redan är installerad tooupdate hello Azure Linux-agenten hello stegen som beskrivs i [uppdatering hello Azure Linux-agenten på en VM toohello senaste versionen från GitHub][virtual-machines-linux-update-agent].</span><span class="sxs-lookup"><span data-stu-id="68e0e-526">If hello agent is already installed, tooupdate hello Azure Linux Agent, do hello steps described in [Update hello Azure Linux Agent on a VM toohello latest version from GitHub][virtual-machines-linux-update-agent].</span></span>

### <span data-ttu-id="68e0e-527"><a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Konfigurera hello-proxy</span><span class="sxs-lookup"><span data-stu-id="68e0e-527"><a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Configure hello proxy</span></span>
<span data-ttu-id="68e0e-528">hello steg du utför tooconfigure hello proxy i Windows skiljer sig från hello sättet att konfigurera hello proxy i Linux.</span><span class="sxs-lookup"><span data-stu-id="68e0e-528">hello steps you take tooconfigure hello proxy in Windows are different from hello way you configure hello proxy in Linux.</span></span>

#### <a name="windows"></a><span data-ttu-id="68e0e-529">Windows</span><span class="sxs-lookup"><span data-stu-id="68e0e-529">Windows</span></span>
<span data-ttu-id="68e0e-530">Proxyinställningar måste ställas in korrekt för hello kontot Lokalt System tooaccess hello Internet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-530">Proxy settings must be set up correctly for hello Local System account tooaccess hello Internet.</span></span> <span data-ttu-id="68e0e-531">Om proxyinställningarna inte har angetts av en Grupprincip kan du konfigurera hello inställningar för hello kontot Lokalt System.</span><span class="sxs-lookup"><span data-stu-id="68e0e-531">If your proxy settings are not set by Group Policy, you can configure hello settings for hello Local System account.</span></span>

1. <span data-ttu-id="68e0e-532">Gå för**starta**, ange **gpedit.msc**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-532">Go too**Start**, enter **gpedit.msc**, and then select **Enter**.</span></span>
2. <span data-ttu-id="68e0e-533">Välj **Datorkonfiguration** > **Administrationsmallar** > **Windows-komponenter** > **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-533">Select **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Internet Explorer**.</span></span> <span data-ttu-id="68e0e-534">Kontrollera att inställningen hello **Se proxy inställningar per dator (i stället för per användare)** är inaktiverad eller inte konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="68e0e-534">Make sure that hello setting **Make proxy settings per-machine (rather than per-user)** is disabled or not configured.</span></span>
3. <span data-ttu-id="68e0e-535">I **Kontrollpanelen**, gå för**nätverks- och delningscenter** > **Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-535">In **Control Panel**, go too**Network and Sharing Center** > **Internet Options**.</span></span>
4. <span data-ttu-id="68e0e-536">På hello **anslutningar** fliken, väljer hello **LAN-inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-536">On hello **Connections** tab, select hello **LAN settings** button.</span></span>
5. <span data-ttu-id="68e0e-537">Rensa hello **automatisk identifiering av inställningar** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="68e0e-537">Clear hello **Automatically detect settings** check box.</span></span>
6. <span data-ttu-id="68e0e-538">Välj hello **använder en proxyserver för nätverket** kryssrutan och sedan ange hello proxyadress och port.</span><span class="sxs-lookup"><span data-stu-id="68e0e-538">Select hello **Use a proxy server for your LAN** check box, and then enter hello proxy address and port.</span></span>
7. <span data-ttu-id="68e0e-539">Välj hello **Avancerat** knappen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-539">Select hello **Advanced** button.</span></span>
8. <span data-ttu-id="68e0e-540">I hello **undantag** ange hello IP-adress **168.63.129.16**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-540">In hello **Exceptions** box, enter hello IP address **168.63.129.16**.</span></span> <span data-ttu-id="68e0e-541">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-541">Select **OK**.</span></span>


#### <a name="linux"></a><span data-ttu-id="68e0e-542">Linux</span><span class="sxs-lookup"><span data-stu-id="68e0e-542">Linux</span></span>
<span data-ttu-id="68e0e-543">Konfigurera rätt hello-proxy i hello konfigurationsfil hello Microsoft Azure-Gästagenten, som finns på \\etc\\waagent.conf.</span><span class="sxs-lookup"><span data-stu-id="68e0e-543">Configure hello correct proxy in hello configuration file of hello Microsoft Azure Guest Agent, which is located at \\etc\\waagent.conf.</span></span>

<span data-ttu-id="68e0e-544">Ange hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="68e0e-544">Set hello following parameters:</span></span>

1.  <span data-ttu-id="68e0e-545">**HTTP-proxyvärd**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-545">**HTTP proxy host**.</span></span> <span data-ttu-id="68e0e-546">Till exempel den för**proxy.corp.local**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-546">For example, set it too**proxy.corp.local**.</span></span>
  ```
  HttpProxy.Host=<proxy host>

  ```
2.  <span data-ttu-id="68e0e-547">**HTTP-Proxyport**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-547">**HTTP proxy port**.</span></span> <span data-ttu-id="68e0e-548">Till exempel den för**80**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-548">For example, set it too**80**.</span></span>
  ```
  HttpProxy.Port=<port of hello proxy host>

  ```
3.  <span data-ttu-id="68e0e-549">Starta om hello-agenten.</span><span class="sxs-lookup"><span data-stu-id="68e0e-549">Restart hello agent.</span></span>

  ```
  sudo service waagent restart
  ```

<span data-ttu-id="68e0e-550">Hej proxyinställningarna i \\etc\\waagent.conf gäller även toohello som krävs för VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="68e0e-550">hello proxy settings in \\etc\\waagent.conf also apply toohello required VM extensions.</span></span> <span data-ttu-id="68e0e-551">Om du vill toouse hello Azure databaser, se till att hello trafik toothese databaser inte gå igenom lokalt intranät.</span><span class="sxs-lookup"><span data-stu-id="68e0e-551">If you want toouse hello Azure repositories, make sure that hello traffic toothese repositories is not going through your on-premises intranet.</span></span> <span data-ttu-id="68e0e-552">Om du har skapat användardefinierade vägar tooenable Tvingad tunneltrafik, se till att du lägger till en väg som dirigerar trafik toohello databaser direkt toohello Internet, och inte via plats-till-plats VPN-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-552">If you created user-defined routes tooenable forced tunneling, make sure that you add a route that routes traffic toohello repositories directly toohello Internet, and not through your site-to-site VPN connection.</span></span>

* <span data-ttu-id="68e0e-553">**SLES**</span><span class="sxs-lookup"><span data-stu-id="68e0e-553">**SLES**</span></span>

  <span data-ttu-id="68e0e-554">Du måste också tooadd vägar för hello IP-adresser som anges i \\etc\\regionserverclnt.cfg.</span><span class="sxs-lookup"><span data-stu-id="68e0e-554">You also need tooadd routes for hello IP addresses listed in \\etc\\regionserverclnt.cfg.</span></span> <span data-ttu-id="68e0e-555">hello visar följande bild ett exempel:</span><span class="sxs-lookup"><span data-stu-id="68e0e-555">hello following figure shows an example:</span></span>

  ![Tvingad tunneltrafik][deployment-guide-figure-50]


* <span data-ttu-id="68e0e-557">**RHEL**</span><span class="sxs-lookup"><span data-stu-id="68e0e-557">**RHEL**</span></span>

  <span data-ttu-id="68e0e-558">Du måste också tooadd vägar för hello IP-adresser för hello värdar som finns i \\etc\\yum.repos.d\\rhui belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="68e0e-558">You also need tooadd routes for hello IP addresses of hello hosts listed in \\etc\\yum.repos.d\\rhui-load-balancers.</span></span> <span data-ttu-id="68e0e-559">Ett exempel finns i hello föregående bild.</span><span class="sxs-lookup"><span data-stu-id="68e0e-559">For an example, see hello preceding figure.</span></span>

<span data-ttu-id="68e0e-560">Mer information om användardefinierade vägar finns [användardefinierade vägar och IP-vidarebefordring][virtual-networks-udr-overview].</span><span class="sxs-lookup"><span data-stu-id="68e0e-560">For more information about user-defined routes, see [User-defined routes and IP forwarding][virtual-networks-udr-overview].</span></span>

### <span data-ttu-id="68e0e-561"><a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Konfigurera hello Azure förbättrad övervakning av tillägget för SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-561"><a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Configure hello Azure Enhanced Monitoring Extension for SAP</span></span>
<span data-ttu-id="68e0e-562">När du har förberett hello VM enligt beskrivningen i [scenarier för distribution av virtuella datorer för SAP på Azure][deployment-guide-3], hello Azure VM-agenten är installerad på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="68e0e-562">When you've prepared hello VM as described in [Deployment scenarios of VMs for SAP on Azure][deployment-guide-3], hello Azure VM Agent is installed on hello virtual machine.</span></span> <span data-ttu-id="68e0e-563">hello nästa steg är toodeploy hello Azure förbättrad övervakning av tillägget för SAP, som finns i hello Azure Tilläggscentrallagret i hello globala Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="68e0e-563">hello next step is toodeploy hello Azure Enhanced Monitoring Extension for SAP, which is available in hello Azure Extension Repository in hello global Azure datacenters.</span></span> <span data-ttu-id="68e0e-564">Mer information finns i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide-9.1].</span><span class="sxs-lookup"><span data-stu-id="68e0e-564">For more information, see [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide-9.1].</span></span>

<span data-ttu-id="68e0e-565">Du kan använda PowerShell eller Azure CLI tooinstall och konfigurera hello Azure förbättrad övervakning av tillägget för SAP.</span><span class="sxs-lookup"><span data-stu-id="68e0e-565">You can use PowerShell or Azure CLI tooinstall and configure hello Azure Enhanced Monitoring Extension for SAP.</span></span> <span data-ttu-id="68e0e-566">tooinstall hello tillägg på en Windows- eller Linux-VM med hjälp av en Windows-dator finns [Azure PowerShell][deployment-guide-4.5.1].</span><span class="sxs-lookup"><span data-stu-id="68e0e-566">tooinstall hello extension on a Windows or Linux VM by using a Windows machine, see [Azure PowerShell][deployment-guide-4.5.1].</span></span> <span data-ttu-id="68e0e-567">tooinstall hello tillägg på en Linux-VM med hjälp av en Linux-skrivbord finns [Azure CLI][deployment-guide-4.5.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-567">tooinstall hello extension on a Linux VM by using a Linux desktop, see [Azure CLI][deployment-guide-4.5.2].</span></span>

#### <span data-ttu-id="68e0e-568"><a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell för Linux och Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="68e0e-568"><a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell for Linux and Windows VMs</span></span>
<span data-ttu-id="68e0e-569">tooinstall hello Azure förbättrad övervakning av tillägget för SAP med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="68e0e-569">tooinstall hello Azure Enhanced Monitoring Extension for SAP by using PowerShell:</span></span>

1. <span data-ttu-id="68e0e-570">Kontrollera att du har installerat hello senaste versionen av hello Azure PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="68e0e-570">Make sure that you have installed hello latest version of hello Azure PowerShell cmdlet.</span></span> <span data-ttu-id="68e0e-571">Mer information finns i [distribution av Azure PowerShell-cmdlets][deployment-guide-4.1].</span><span class="sxs-lookup"><span data-stu-id="68e0e-571">For more information, see [Deploying Azure PowerShell cmdlets][deployment-guide-4.1].</span></span>  
2. <span data-ttu-id="68e0e-572">Kör följande PowerShell-cmdleten hello.</span><span class="sxs-lookup"><span data-stu-id="68e0e-572">Run hello following PowerShell cmdlet.</span></span>
  <span data-ttu-id="68e0e-573">En lista över tillgängliga miljöer, kör `commandlet Get-AzureRmEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="68e0e-573">For a list of available environments, run `commandlet Get-AzureRmEnvironment`.</span></span> <span data-ttu-id="68e0e-574">Om du vill toouse offentliga Azure måste din miljö är **AzureCloud**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-574">If you want toouse public Azure, your environment is **AzureCloud**.</span></span> <span data-ttu-id="68e0e-575">Azure i Kina, Välj **AzureChinaCloud**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-575">For Azure in China, select **AzureChinaCloud**.</span></span>


      ```powershell
      $env = Get-AzureRmEnvironment -Name <name of hello environment>
      Login-AzureRmAccount -Environment $env
      Set-AzureRmContext -SubscriptionName <subscription name>

      Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
      ```

<span data-ttu-id="68e0e-576">När du anger data för ditt konto och identifiera hello Azure-dator, distribuerar hello krävs tillägg hello skript och aktiverar hello krävs funktioner.</span><span class="sxs-lookup"><span data-stu-id="68e0e-576">After you enter your account data and identify hello Azure virtual machine, hello script deploys hello required extensions and enables hello required features.</span></span> <span data-ttu-id="68e0e-577">Det kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="68e0e-577">This can take several minutes.</span></span>
<span data-ttu-id="68e0e-578">Mer information om `Set-AzureRmVMAEMExtension`, se [Set AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].</span><span class="sxs-lookup"><span data-stu-id="68e0e-578">For more information about `Set-AzureRmVMAEMExtension`, see [Set-AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].</span></span>

![SAP-specifika har körts Azure cmdlet Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

<span data-ttu-id="68e0e-580">Hej `Set-AzureRmVMAEMExtension` konfiguration har steg tooconfigure av alla hello-värden som övervakning för SAP.</span><span class="sxs-lookup"><span data-stu-id="68e0e-580">hello `Set-AzureRmVMAEMExtension` configuration does all hello steps tooconfigure host monitoring for SAP.</span></span>

<span data-ttu-id="68e0e-581">utdata för hello skriptet innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="68e0e-581">hello script output includes hello following information:</span></span>

* <span data-ttu-id="68e0e-582">Bekräfta att övervakning för grundläggande hello VHD (med hello OS) och alla ytterligare virtuella hårddiskar monterade toohello VM har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="68e0e-582">Confirmation that monitoring for hello base VHD (with hello OS) and all additional VHDs mounted toohello VM has been configured.</span></span>
* <span data-ttu-id="68e0e-583">följande två hälsningsmeddelande bekräfta hello konfigurationen av Storage-mätvärden för ett visst lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="68e0e-583">hello next two messages confirm hello configuration of Storage Metrics for a specific storage account.</span></span>
* <span data-ttu-id="68e0e-584">En rad med utdata ger hello status för hello faktiska uppdateringen av hello övervakningskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-584">One line of output gives hello status of hello actual update of hello monitoring configuration.</span></span>
* <span data-ttu-id="68e0e-585">Ytterligare en rad i utdata bekräftar att hello-konfigurationen har distribuerats eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="68e0e-585">Another line of output confirms that hello configuration has been deployed or updated.</span></span>
* <span data-ttu-id="68e0e-586">hello sista raden i utdata visas i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="68e0e-586">hello last line of output is informational.</span></span> <span data-ttu-id="68e0e-587">Den visar alternativen för tester hello övervakningskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-587">It shows your options for testing hello monitoring configuration.</span></span>

<span data-ttu-id="68e0e-588">toocheck att alla steg i Azure-förbättrad övervakning har utförts korrekt och att hello Azure-infrastrukturen tillhandahåller hello nödvändiga data fortsätta med hello beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP, enligt beskrivningen i [Beredskapskontrollen för Azure-förbättrad övervakning för SAP][deployment-guide-5.1].</span><span class="sxs-lookup"><span data-stu-id="68e0e-588">toocheck that all steps of Azure Enhanced Monitoring have been executed successfully, and that hello Azure Infrastructure provides hello necessary data, proceed with hello readiness check for hello Azure Enhanced Monitoring Extension for SAP, as described in [Readiness check for Azure Enhanced Monitoring for SAP][deployment-guide-5.1].</span></span> <span data-ttu-id="68e0e-589">Vänta 15 30 minuter för Azure-diagnostik toocollect hello relevanta data.</span><span class="sxs-lookup"><span data-stu-id="68e0e-589">Wait 15-30 minutes for Azure Diagnostics toocollect hello relevant data.</span></span>

#### <span data-ttu-id="68e0e-590"><a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="68e0e-590"><a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI for Linux VMs</span></span>
<span data-ttu-id="68e0e-591">tooinstall hello Azure förbättrad övervakning av tillägget för SAP med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="68e0e-591">tooinstall hello Azure Enhanced Monitoring Extension for SAP by using Azure CLI:</span></span>

1. <span data-ttu-id="68e0e-592">Installera Azure CLI, enligt beskrivningen i [installera hello Azure CLI][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="68e0e-592">Install Azure CLI, as described in [Install hello Azure CLI][azure-cli].</span></span>
2. <span data-ttu-id="68e0e-593">Logga in med ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="68e0e-593">Sign in with your Azure account:</span></span>

  ```
  azure login
  ```

3. <span data-ttu-id="68e0e-594">Växla tooAzure Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="68e0e-594">Switch tooAzure Resource Manager mode:</span></span>

  ```
  azure config mode arm
  ```

4. <span data-ttu-id="68e0e-595">Aktivera Azure utökad övervakning:</span><span class="sxs-lookup"><span data-stu-id="68e0e-595">Enable Azure Enhanced Monitoring:</span></span>

  ```
  azure vm enable-aem <resource-group-name> <vm-name>
  ```

5. <span data-ttu-id="68e0e-596">Kontrollera att hello Azure förbättrad övervakning av tillägget är aktiv på hello virtuella Azure Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-596">Verify that hello Azure Enhanced Monitoring Extension is active on hello Azure Linux VM.</span></span> <span data-ttu-id="68e0e-597">Kontrollera om hello filen \\var\\lib\\AzureEnhancedMonitor\\PerfCounters finns.</span><span class="sxs-lookup"><span data-stu-id="68e0e-597">Check whether hello file \\var\\lib\\AzureEnhancedMonitor\\PerfCounters exists.</span></span> <span data-ttu-id="68e0e-598">Kör det här kommandot toodisplay information som samlas in av hello Azure förbättrad övervakaren om den finns i en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="68e0e-598">If it exists, at a command prompt, run this command toodisplay information collected by hello Azure Enhanced Monitor:</span></span>
```
cat /var/lib/AzureEnhancedMonitor/PerfCounters
```

<span data-ttu-id="68e0e-599">hello utdata ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="68e0e-599">hello output looks like this:</span></span>
```
2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
…
…
```

## <span data-ttu-id="68e0e-600"><a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Kontroller och felsökning för slutpunkt till slutpunkt övervakning</span><span class="sxs-lookup"><span data-stu-id="68e0e-600"><a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Checks and troubleshooting for end-to-end monitoring</span></span>
<span data-ttu-id="68e0e-601">När du har distribuerat den virtuella Azure-datorn och konfigurera hello relevanta Azure övervakade infrastrukturen, kontrollerar du om alla hello komponenter i hello Azure förbättrad övervakning tillägg fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="68e0e-601">After you have deployed your Azure VM and set up hello relevant Azure monitoring infrastructure, check whether all hello components of hello Azure Enhanced Monitoring Extension are working as expected.</span></span>

<span data-ttu-id="68e0e-602">Köra hello beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP enligt beskrivningen i [beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-5.1].</span><span class="sxs-lookup"><span data-stu-id="68e0e-602">Run hello readiness check for hello Azure Enhanced Monitoring Extension for SAP as described in [Readiness check for hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-5.1].</span></span> <span data-ttu-id="68e0e-603">Om alla resultat av beredskap är positivt och alla relevanta prestandaräknare visas OK, Azure övervakning har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="68e0e-603">If all readiness check results are positive and all relevant performance counters appear OK, Azure monitoring successfully set up.</span></span> <span data-ttu-id="68e0e-604">Du kan fortsätta med hello installation av SAP Värdagenten som beskrivs i hello SAP anteckningar i [SAP resurser][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-604">You can proceed with hello installation of SAP Host Agent described in hello SAP Notes in [SAP resources][deployment-guide-2.2].</span></span> <span data-ttu-id="68e0e-605">Om hello beredskapskontroll indikerar att räknare saknas, köra hello hälsokontrollen för hello Azure övervakade infrastrukturen enligt beskrivningen i [hälsokontrollen för Azure infrastructure övervakningskonfigurationen] [ deployment-guide-5.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-605">If hello readiness check indicates that counters are missing, run hello health check for hello Azure monitoring infrastructure, as described in [Health check for Azure monitoring infrastructure configuration][deployment-guide-5.2].</span></span> <span data-ttu-id="68e0e-606">Fler alternativ för felsökning, se [felsökning Azure-övervakning för SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-606">For more troubleshooting options, see [Troubleshooting Azure monitoring for SAP][deployment-guide-5.3].</span></span>

### <span data-ttu-id="68e0e-607"><a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-607"><a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Readiness check for hello Azure Enhanced Monitoring Extension for SAP</span></span>
<span data-ttu-id="68e0e-608">Den här kontrollen ser till att alla prestandamått som visas i din SAP-program som tillhandahålls av hello underliggande Azure övervakning av infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="68e0e-608">This check makes sure that all performance metrics that appear inside your SAP application are provided by hello underlying Azure monitoring infrastructure.</span></span>

#### <a name="run-hello-readiness-check-on-a-windows-vm"></a><span data-ttu-id="68e0e-609">Kör hello beredskapskontroll på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="68e0e-609">Run hello readiness check on a Windows VM</span></span>

1.  <span data-ttu-id="68e0e-610">Logga in toohello virtuella Azure-datorn (med ett administratörskonto är inte nödvändigt).</span><span class="sxs-lookup"><span data-stu-id="68e0e-610">Sign in toohello Azure virtual machine (using an admin account is not necessary).</span></span>
2.  <span data-ttu-id="68e0e-611">Öppna ett kommandotolksfönster.</span><span class="sxs-lookup"><span data-stu-id="68e0e-611">Open a Command Prompt window.</span></span>
3.  <span data-ttu-id="68e0e-612">Ändra hello directory toohello installationsmappen för hello Azure förbättrad övervakning av tillägget för SAP Kommandotolken hello: C:\\paket\\plugin-program\\ Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version >\\släppa</span><span class="sxs-lookup"><span data-stu-id="68e0e-612">At hello command prompt, change hello directory toohello installation folder of hello Azure Enhanced Monitoring Extension for SAP: C:\\Packages\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version>\\drop</span></span>

  <span data-ttu-id="68e0e-613">Hej *version* i hello sökvägen toohello övervakning tillägget kan variera.</span><span class="sxs-lookup"><span data-stu-id="68e0e-613">hello *version* in hello path toohello monitoring extension might vary.</span></span> <span data-ttu-id="68e0e-614">Om du ser mappar för flera versioner av hello övervakning tillägget hello-installationsmappen, kontrollera hello konfigurationen av hello AzureEnhancedMonitoring Windows-tjänsten och sedan växla toohello mapp anges som *sökvägen tooexecutable* .</span><span class="sxs-lookup"><span data-stu-id="68e0e-614">If you see folders for multiple versions of hello monitoring extension in hello installation folder, check hello configuration of hello AzureEnhancedMonitoring Windows service, and then switch toohello folder indicated as *Path tooexecutable*.</span></span>

  ![Egenskaper för tjänsten som körs hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-figure-1000]

4.  <span data-ttu-id="68e0e-616">Kommandotolken hello kör **azperflib.exe** utan några parametrar.</span><span class="sxs-lookup"><span data-stu-id="68e0e-616">At hello command prompt, run **azperflib.exe** without any parameters.</span></span>

  > [!NOTE]
  > <span data-ttu-id="68e0e-617">Azperflib.exe körs i en slinga och uppdaterar hello samlas in räknare var 60: e sekund.</span><span class="sxs-lookup"><span data-stu-id="68e0e-617">Azperflib.exe runs in a loop and updates hello collected counters every 60 seconds.</span></span> <span data-ttu-id="68e0e-618">tooend hello slinga, Stäng hello Kommandotolkens fönster.</span><span class="sxs-lookup"><span data-stu-id="68e0e-618">tooend hello loop, close hello Command Prompt window.</span></span>
  >
  >

<span data-ttu-id="68e0e-619">Om hello Azure förbättrad övervakning av tillägget inte har installerats eller hello AzureEnhancedMonitoring tjänsten inte körs, har hello-tillägget inte konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="68e0e-619">If hello Azure Enhanced Monitoring Extension is not installed, or hello AzureEnhancedMonitoring service is not running, hello extension has not been configured correctly.</span></span> <span data-ttu-id="68e0e-620">Detaljerad information om hur toodeploy hello tillägg finns [felsökning hello Azure övervakade infrastrukturen för SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-620">For detailed information about how toodeploy hello extension, see [Troubleshooting hello Azure monitoring infrastructure for SAP][deployment-guide-5.3].</span></span>

##### <a name="check-hello-output-of-azperflibexe"></a><span data-ttu-id="68e0e-621">Kontrollera hello utdata från azperflib.exe</span><span class="sxs-lookup"><span data-stu-id="68e0e-621">Check hello output of azperflib.exe</span></span>
<span data-ttu-id="68e0e-622">Azperflib.exe utdata visar alla fyllts i Azure prestandaräknare för SAP.</span><span class="sxs-lookup"><span data-stu-id="68e0e-622">Azperflib.exe output shows all populated Azure performance counters for SAP.</span></span> <span data-ttu-id="68e0e-623">Längst ned hello hello lista över insamlade räknare visa en sammanfattning och hälsa indikator hello status för övervakning av Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-623">At hello bottom of hello list of collected counters, a summary and health indicator show hello status of Azure monitoring.</span></span>

<span data-ttu-id="68e0e-624">![Utdata för hälsokontrollen genom att köra azperflib.exe som anger att det inte finns några problem][deployment-guide-figure-1100]
<a name="figure-11"></a></span><span class="sxs-lookup"><span data-stu-id="68e0e-624">![Output of health check by executing azperflib.exe, which indicates that no problems exist][deployment-guide-figure-1100]
<a name="figure-11"></a></span></span>

<span data-ttu-id="68e0e-625">Kontrollera hello resultatet som returneras för hello **räknare totalt** utdata som rapporteras som tom och för **hälsostatus**, visas i hello föregående bild.</span><span class="sxs-lookup"><span data-stu-id="68e0e-625">Check hello result returned for hello **Counters total** output, which is reported as empty, and for **Health status**, shown in hello preceding figure.</span></span>

<span data-ttu-id="68e0e-626">Tolka hello värden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="68e0e-626">Interpret hello resulting values as follows:</span></span>

| <span data-ttu-id="68e0e-627">Azperflib.exe resultatvärden</span><span class="sxs-lookup"><span data-stu-id="68e0e-627">Azperflib.exe result values</span></span> | <span data-ttu-id="68e0e-628">Azure övervakningsstatusen hälsa</span><span class="sxs-lookup"><span data-stu-id="68e0e-628">Azure monitoring health status</span></span> |
| --- | --- |
| <span data-ttu-id="68e0e-629">**API-anrop - inte tillgänglig**</span><span class="sxs-lookup"><span data-stu-id="68e0e-629">**API Calls - not available**</span></span> | <span data-ttu-id="68e0e-630">Räknare som inte är tillgänglig kan vara antingen inte tillämpligt toohello virtuella datorkonfigurationen eller är fel.</span><span class="sxs-lookup"><span data-stu-id="68e0e-630">Counters that are not available might be either not applicable toohello virtual machine configuration, or are errors.</span></span> <span data-ttu-id="68e0e-631">Se **hälsostatus**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-631">See **Health status**.</span></span> |
| <span data-ttu-id="68e0e-632">**Prestandaräknare för totala – tomt**</span><span class="sxs-lookup"><span data-stu-id="68e0e-632">**Counters total - empty**</span></span> |<span data-ttu-id="68e0e-633">hello följande två Azure storage-räknare kan vara tom:</span><span class="sxs-lookup"><span data-stu-id="68e0e-633">hello following two Azure storage counters can be empty:</span></span> <ul><li><span data-ttu-id="68e0e-634">Op svarstid för läsning Server msek</span><span class="sxs-lookup"><span data-stu-id="68e0e-634">Storage Read Op Latency Server msec</span></span></li><li><span data-ttu-id="68e0e-635">Op svarstid för läsning E2E msek</span><span class="sxs-lookup"><span data-stu-id="68e0e-635">Storage Read Op Latency E2E msec</span></span></li></ul><span data-ttu-id="68e0e-636">Alla räknare måste ha värden.</span><span class="sxs-lookup"><span data-stu-id="68e0e-636">All other counters must have values.</span></span> |
| <span data-ttu-id="68e0e-637">**Hälsotillstånd**</span><span class="sxs-lookup"><span data-stu-id="68e0e-637">**Health status**</span></span> |<span data-ttu-id="68e0e-638">Endast OK om returnerar status visas **OK**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-638">Only OK if return status shows **OK**.</span></span> |
| <span data-ttu-id="68e0e-639">**Diagnostik**</span><span class="sxs-lookup"><span data-stu-id="68e0e-639">**Diagnostics**</span></span> |<span data-ttu-id="68e0e-640">Detaljerad information om hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="68e0e-640">Detailed information about health status.</span></span> |

<span data-ttu-id="68e0e-641">Om hello **hälsostatus** värdet är inte **OK**, följer du anvisningarna hello i [hälsokontrollen för Azure infrastructure övervakningskonfigurationen] [ deployment-guide-5.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-641">If hello **Health status** value is not **OK**, follow hello instructions in [Health check for Azure monitoring infrastructure configuration][deployment-guide-5.2].</span></span>

#### <a name="run-hello-readiness-check-on-a-linux-vm"></a><span data-ttu-id="68e0e-642">Kör hello beredskapskontroll på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="68e0e-642">Run hello readiness check on a Linux VM</span></span>

1.  <span data-ttu-id="68e0e-643">Ansluta toohello Azure-dator med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="68e0e-643">Connect toohello Azure Virtual Machine by using SSH.</span></span>

2.  <span data-ttu-id="68e0e-644">Kontrollera hello utdata från hello Azure förbättrad övervakning av tillägget.</span><span class="sxs-lookup"><span data-stu-id="68e0e-644">Check hello output of hello Azure Enhanced Monitoring Extension.</span></span>

  <span data-ttu-id="68e0e-645">a.</span><span class="sxs-lookup"><span data-stu-id="68e0e-645">a.</span></span>  <span data-ttu-id="68e0e-646">Kör `more /var/lib/AzureEnhancedMonitor/PerfCounters`</span><span class="sxs-lookup"><span data-stu-id="68e0e-646">Run `more /var/lib/AzureEnhancedMonitor/PerfCounters`</span></span>

   <span data-ttu-id="68e0e-647">**Förväntat resultat**: returnerar lista över prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="68e0e-647">**Expected result**: Returns list of performance counters.</span></span> <span data-ttu-id="68e0e-648">hello-filen får inte vara tomt.</span><span class="sxs-lookup"><span data-stu-id="68e0e-648">hello file should not be empty.</span></span>

 <span data-ttu-id="68e0e-649">b.</span><span class="sxs-lookup"><span data-stu-id="68e0e-649">b.</span></span> <span data-ttu-id="68e0e-650">Kör `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error`</span><span class="sxs-lookup"><span data-stu-id="68e0e-650">Run `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error`</span></span>

   <span data-ttu-id="68e0e-651">**Förväntat resultat**: returnerar en rad där hello fel är **ingen**, till exempel **3; config; Fel; 0; 0; Ingen; 1456416792; 0 tst-servercs;**</span><span class="sxs-lookup"><span data-stu-id="68e0e-651">**Expected result**: Returns one line where hello error is **none**, for example, **3;config;Error;;0;0;none;0;1456416792;tst-servercs;**</span></span>

  <span data-ttu-id="68e0e-652">c.</span><span class="sxs-lookup"><span data-stu-id="68e0e-652">c.</span></span> <span data-ttu-id="68e0e-653">Kör `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord`</span><span class="sxs-lookup"><span data-stu-id="68e0e-653">Run `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord`</span></span>

    <span data-ttu-id="68e0e-654">**Förväntat resultat**: returnerar som tom eller finns inte.</span><span class="sxs-lookup"><span data-stu-id="68e0e-654">**Expected result**: Returns as empty or does not exist.</span></span>

<span data-ttu-id="68e0e-655">Om hello föregående Kontrollera inte lyckades kan du köra dessa ytterligare kontroller:</span><span class="sxs-lookup"><span data-stu-id="68e0e-655">If hello preceding check was not successful, run these additional checks:</span></span>

1.  <span data-ttu-id="68e0e-656">Kontrollera att hello waagent är installerat och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="68e0e-656">Make sure that hello waagent is installed and enabled.</span></span>

  <span data-ttu-id="68e0e-657">a.</span><span class="sxs-lookup"><span data-stu-id="68e0e-657">a.</span></span>  <span data-ttu-id="68e0e-658">Kör `sudo ls -al /var/lib/waagent/`</span><span class="sxs-lookup"><span data-stu-id="68e0e-658">Run `sudo ls -al /var/lib/waagent/`</span></span>

      <span data-ttu-id="68e0e-659">**Förväntat resultat**: visas hello innehåll för hello waagent katalog.</span><span class="sxs-lookup"><span data-stu-id="68e0e-659">**Expected result**: Lists hello content of hello waagent directory.</span></span>

  <span data-ttu-id="68e0e-660">b.</span><span class="sxs-lookup"><span data-stu-id="68e0e-660">b.</span></span>  <span data-ttu-id="68e0e-661">Kör `ps -ax | grep waagent`</span><span class="sxs-lookup"><span data-stu-id="68e0e-661">Run `ps -ax | grep waagent`</span></span>

   <span data-ttu-id="68e0e-662">**Förväntat resultat**: Visar en post som liknar:`python /usr/sbin/waagent -daemon`</span><span class="sxs-lookup"><span data-stu-id="68e0e-662">**Expected result**: Displays one entry similar to: `python /usr/sbin/waagent -daemon`</span></span>

2. <span data-ttu-id="68e0e-663">Se till att hello Linux diagnostiska tillägget är installerat och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="68e0e-663">Make sure that hello Linux Diagnostic Extension is installed and enabled.</span></span>

  <span data-ttu-id="68e0e-664">a.</span><span class="sxs-lookup"><span data-stu-id="68e0e-664">a.</span></span>  <span data-ttu-id="68e0e-665">Kör `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-'`</span><span class="sxs-lookup"><span data-stu-id="68e0e-665">Run `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-'`</span></span>

   <span data-ttu-id="68e0e-666">**Förväntat resultat**: visas hello innehåll för hello Linux diagnostiska tillägget katalog.</span><span class="sxs-lookup"><span data-stu-id="68e0e-666">**Expected result**: Lists hello content of hello Linux Diagnostic Extension directory.</span></span>

 <span data-ttu-id="68e0e-667">b.</span><span class="sxs-lookup"><span data-stu-id="68e0e-667">b.</span></span> <span data-ttu-id="68e0e-668">Kör `ps -ax | grep diagnostic`</span><span class="sxs-lookup"><span data-stu-id="68e0e-668">Run `ps -ax | grep diagnostic`</span></span>

   <span data-ttu-id="68e0e-669">**Förväntat resultat**: Visar en post som liknar:`python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py -daemon`</span><span class="sxs-lookup"><span data-stu-id="68e0e-669">**Expected result**: Displays one entry similar to: `python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py -daemon`</span></span>

3.   <span data-ttu-id="68e0e-670">Kontrollera att hello Azure förbättrad övervakning av tillägget är installerad och körs.</span><span class="sxs-lookup"><span data-stu-id="68e0e-670">Make sure that hello Azure Enhanced Monitoring Extension is installed and running.</span></span>

  <span data-ttu-id="68e0e-671">a.</span><span class="sxs-lookup"><span data-stu-id="68e0e-671">a.</span></span>  <span data-ttu-id="68e0e-672">Kör `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-/'`</span><span class="sxs-lookup"><span data-stu-id="68e0e-672">Run `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-/'`</span></span>

    <span data-ttu-id="68e0e-673">**Förväntat resultat**: Visar hello innehållet i katalog för hello Azure förbättrad övervakning av tillägget.</span><span class="sxs-lookup"><span data-stu-id="68e0e-673">**Expected result**: Lists hello content of hello Azure Enhanced Monitoring Extension directory.</span></span>

  <span data-ttu-id="68e0e-674">b.</span><span class="sxs-lookup"><span data-stu-id="68e0e-674">b.</span></span> <span data-ttu-id="68e0e-675">Kör `ps -ax | grep AzureEnhanced`</span><span class="sxs-lookup"><span data-stu-id="68e0e-675">Run `ps -ax | grep AzureEnhanced`</span></span>

     <span data-ttu-id="68e0e-676">**Förväntat resultat**: Visar en post som liknar:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`</span><span class="sxs-lookup"><span data-stu-id="68e0e-676">**Expected result**: Displays one entry similar to: `python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`</span></span>

3. <span data-ttu-id="68e0e-677">Installera SAP Värdagenten enligt beskrivningen i SAP-kommentar [1031096], och kontrollera hello utdata från `saposcol`.</span><span class="sxs-lookup"><span data-stu-id="68e0e-677">Install SAP Host Agent as described in SAP Note [1031096], and check hello output of `saposcol`.</span></span>

  <span data-ttu-id="68e0e-678">a.</span><span class="sxs-lookup"><span data-stu-id="68e0e-678">a.</span></span>  <span data-ttu-id="68e0e-679">Kör `/usr/sap/hostctrl/exe/saposcol -d`</span><span class="sxs-lookup"><span data-stu-id="68e0e-679">Run `/usr/sap/hostctrl/exe/saposcol -d`</span></span>

  <span data-ttu-id="68e0e-680">b.</span><span class="sxs-lookup"><span data-stu-id="68e0e-680">b.</span></span>  <span data-ttu-id="68e0e-681">Kör `dump ccm`</span><span class="sxs-lookup"><span data-stu-id="68e0e-681">Run `dump ccm`</span></span>

  <span data-ttu-id="68e0e-682">c.</span><span class="sxs-lookup"><span data-stu-id="68e0e-682">c.</span></span>  <span data-ttu-id="68e0e-683">Kontrollera om hello **Virtualization_Configuration\Enhanced övervakning åtkomst** mått är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-683">Check whether hello **Virtualization_Configuration\Enhanced Monitoring Access** metric is **true**.</span></span>

<span data-ttu-id="68e0e-684">Om du redan har en programserver för SAP NetWeaver ABAP installerat öppna transaktionen ST06 och kontrollera om utökad övervakning är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="68e0e-684">If you already have an SAP NetWeaver ABAP application server installed, open transaction ST06 and check whether enhanced monitoring is enabled.</span></span>

<span data-ttu-id="68e0e-685">Om någon av de här kontrollerna misslyckas och detaljerad information om hur tooredeploy hello tillägget ser [felsökning hello Azure övervakade infrastrukturen för SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-685">If any of these checks fail, and for detailed information about how tooredeploy hello extension, see [Troubleshooting hello Azure monitoring infrastructure for SAP][deployment-guide-5.3].</span></span>

### <span data-ttu-id="68e0e-686"><a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Hälsotillstånd söka efter hello Azure övervakningskonfigurationen infrastruktur</span><span class="sxs-lookup"><span data-stu-id="68e0e-686"><a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Health check for hello Azure monitoring infrastructure configuration</span></span>
<span data-ttu-id="68e0e-687">Om några av hello övervakningsdata inte levereras korrekt enligt hello test som beskrivs i [beredskapskontrollen för Azure-förbättrad övervakning för SAP][deployment-guide-5.1]kör hello `Test-AzureRmVMAEMExtension` cmdlet toocheck Om hello Azure övervakning av infrastruktur och hello övervakning tillägget för SAP är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="68e0e-687">If some of hello monitoring data is not delivered correctly as indicated by hello test described in [Readiness check for Azure Enhanced Monitoring for SAP][deployment-guide-5.1], run hello `Test-AzureRmVMAEMExtension` cmdlet toocheck whether hello Azure monitoring infrastructure and hello monitoring extension for SAP are configured correctly.</span></span>

1.  <span data-ttu-id="68e0e-688">Kontrollera att du har installerat hello senaste versionen av hello Azure PowerShell-cmdleten, enligt beskrivningen i [distribution av Azure PowerShell-cmdlets][deployment-guide-4.1].</span><span class="sxs-lookup"><span data-stu-id="68e0e-688">Make sure that you have installed hello latest version of hello Azure PowerShell cmdlet, as described in [Deploying Azure PowerShell cmdlets][deployment-guide-4.1].</span></span>
2.  <span data-ttu-id="68e0e-689">Kör följande PowerShell-cmdleten hello.</span><span class="sxs-lookup"><span data-stu-id="68e0e-689">Run hello following PowerShell cmdlet.</span></span> <span data-ttu-id="68e0e-690">En lista över tillgängliga miljöer kör hello cmdlet `Get-AzureRmEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="68e0e-690">For a list of available environments, run hello cmdlet `Get-AzureRmEnvironment`.</span></span> <span data-ttu-id="68e0e-691">toouse offentlig Azure, Välj hello **AzureCloud** miljö.</span><span class="sxs-lookup"><span data-stu-id="68e0e-691">toouse public Azure, select hello **AzureCloud** environment.</span></span> <span data-ttu-id="68e0e-692">Azure i Kina, Välj **AzureChinaCloud**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-692">For Azure in China, select **AzureChinaCloud**.</span></span>
  ```powershell
  $env = Get-AzureRmEnvironment -Name <name of hello environment>
  Login-AzureRmAccount -Environment $env
  Set-AzureRmContext -SubscriptionName <subscription name>
  Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
  ```

3.  <span data-ttu-id="68e0e-693">Ange kontodata för och identifiera hello virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="68e0e-693">Enter your account data and identify hello Azure virtual machine.</span></span>

  ![Inkommande sida i SAP-specifika Azure cmdleten Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

4. <span data-ttu-id="68e0e-695">hello skript tester hello konfigurationen av hello virtuella dator som du väljer.</span><span class="sxs-lookup"><span data-stu-id="68e0e-695">hello script tests hello configuration of hello virtual machine you select.</span></span>

  ![Utdata från lyckad test av hello Azure övervakade infrastrukturen för SAP][deployment-guide-figure-1300]

<span data-ttu-id="68e0e-697">Se till att varje hälsotillstånd resultat är **OK**.</span><span class="sxs-lookup"><span data-stu-id="68e0e-697">Make sure that every health check result is **OK**.</span></span> <span data-ttu-id="68e0e-698">Om vissa kontroller inte visas **OK**, köra hello uppdatera cmdlet enligt beskrivningen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="68e0e-698">If some checks do not display **OK**, run hello update cmdlet as described in [Configure hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="68e0e-699">Vänta 15 minuter och upprepa hello kontroller som beskrivs i [beredskapskontrollen för Azure-förbättrad övervakning för SAP] [ deployment-guide-5.1] och [hälsokontrollen för Azure-infrastrukturen konfigurationen av övervakning] [deployment-guide-5.2].</span><span class="sxs-lookup"><span data-stu-id="68e0e-699">Wait 15 minutes, and repeat hello checks described in [Readiness check for Azure Enhanced Monitoring for SAP][deployment-guide-5.1] and [Health check for Azure Monitoring Infrastructure Configuration][deployment-guide-5.2].</span></span> <span data-ttu-id="68e0e-700">Om hello kontrollerar fortfarande tyda på ett problem med vissa eller alla räknare, se [felsökning hello Azure övervakade infrastrukturen för SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-700">If hello checks still indicate a problem with some or all counters, see [Troubleshooting hello Azure monitoring infrastructure for SAP][deployment-guide-5.3].</span></span>

### <span data-ttu-id="68e0e-701"><a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Felsöka hello Azure övervakade infrastrukturen för SAP</span><span class="sxs-lookup"><span data-stu-id="68e0e-701"><a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Troubleshooting hello Azure monitoring infrastructure for SAP</span></span>

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] <span data-ttu-id="68e0e-703">Azure prestandaräknare visas inte alls</span><span class="sxs-lookup"><span data-stu-id="68e0e-703">Azure performance counters do not show up at all</span></span>
<span data-ttu-id="68e0e-704">Hej AzureEnhancedMonitoring Windows-tjänsten samlar in prestandamått i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-704">hello AzureEnhancedMonitoring Windows service collects performance metrics in Azure.</span></span> <span data-ttu-id="68e0e-705">Om hello-tjänsten inte har installerats på rätt sätt eller om den inte körs i den virtuella datorn, kan inga prestandamått samlas in.</span><span class="sxs-lookup"><span data-stu-id="68e0e-705">If hello service has not been installed correctly or if it is not running in your VM, no performance metrics can be collected.</span></span>

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a><span data-ttu-id="68e0e-706">hello installationskatalog hello Azure förbättrad övervakning av tillägget är tom</span><span class="sxs-lookup"><span data-stu-id="68e0e-706">hello installation directory of hello Azure Enhanced Monitoring Extension is empty</span></span>

###### <a name="issue"></a><span data-ttu-id="68e0e-707">Problem</span><span class="sxs-lookup"><span data-stu-id="68e0e-707">Issue</span></span>
<span data-ttu-id="68e0e-708">hello-installationskatalogen C:\\paket\\plugin-program\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version >\\släpp är tom.</span><span class="sxs-lookup"><span data-stu-id="68e0e-708">hello installation directory C:\\Packages\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version>\\drop is empty.</span></span>

###### <a name="solution"></a><span data-ttu-id="68e0e-709">Lösning</span><span class="sxs-lookup"><span data-stu-id="68e0e-709">Solution</span></span>
<span data-ttu-id="68e0e-710">hello-tillägget har inte installerats.</span><span class="sxs-lookup"><span data-stu-id="68e0e-710">hello extension is not installed.</span></span> <span data-ttu-id="68e0e-711">Avgöra om detta är en proxy-problemet (enligt beskrivningen ovan).</span><span class="sxs-lookup"><span data-stu-id="68e0e-711">Determine whether this is a proxy issue (as described earlier).</span></span> <span data-ttu-id="68e0e-712">Du kan behöva toorestart hello datorn eller kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript.</span><span class="sxs-lookup"><span data-stu-id="68e0e-712">You might need toorestart hello machine or rerun hello `Set-AzureRmVMAEMExtension` configuration script.</span></span>

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a><span data-ttu-id="68e0e-713">Tjänsten för Azure-förbättrad övervakning finns inte</span><span class="sxs-lookup"><span data-stu-id="68e0e-713">Service for Azure Enhanced Monitoring does not exist</span></span>

###### <a name="issue"></a><span data-ttu-id="68e0e-714">Problem</span><span class="sxs-lookup"><span data-stu-id="68e0e-714">Issue</span></span>
<span data-ttu-id="68e0e-715">Hej AzureEnhancedMonitoring Windows-tjänsten finns inte.</span><span class="sxs-lookup"><span data-stu-id="68e0e-715">hello AzureEnhancedMonitoring Windows service does not exist.</span></span>

<span data-ttu-id="68e0e-716">Azperflib.exe utdata genererar ett fel:</span><span class="sxs-lookup"><span data-stu-id="68e0e-716">Azperflib.exe output throws an error:</span></span>

<span data-ttu-id="68e0e-717">![Körningen av azperflib.exe anger att hello tjänst av hello Azure förbättrad övervakning av tillägget för SAP inte körs][deployment-guide-figure-1400]
<a name="figure-14"></a></span><span class="sxs-lookup"><span data-stu-id="68e0e-717">![Execution of azperflib.exe indicates that hello service of hello Azure Enhanced Monitoring Extension for SAP is not running][deployment-guide-figure-1400]
<a name="figure-14"></a></span></span>

###### <a name="solution"></a><span data-ttu-id="68e0e-718">Lösning</span><span class="sxs-lookup"><span data-stu-id="68e0e-718">Solution</span></span>
<span data-ttu-id="68e0e-719">Om hello-tjänsten inte finns, har hello Azure förbättrad övervakning av tillägget för SAP inte installerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="68e0e-719">If hello service does not exist, hello Azure Enhanced Monitoring Extension for SAP has not been installed correctly.</span></span> <span data-ttu-id="68e0e-720">Distribuera om hello-tillägget med hjälp av stegen för distributionsscenariot i hello [scenarier för distribution av virtuella datorer för SAP i Azure][deployment-guide-3].</span><span class="sxs-lookup"><span data-stu-id="68e0e-720">Redeploy hello extension by using hello steps described for your deployment scenario in [Deployment scenarios of VMs for SAP in Azure][deployment-guide-3].</span></span>

<span data-ttu-id="68e0e-721">När du har distribuerat hello-tillägg, efter en timme, Kontrollera igen om hello Azure prestandaräknare finns i hello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="68e0e-721">After you deploy hello extension, after one hour, check again whether hello Azure performance counters are provided in hello Azure VM.</span></span>

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-toostart"></a><span data-ttu-id="68e0e-722">Tjänsten för Azure-förbättrad övervakning finns, men inte toostart</span><span class="sxs-lookup"><span data-stu-id="68e0e-722">Service for Azure Enhanced Monitoring exists, but fails toostart</span></span>

###### <a name="issue"></a><span data-ttu-id="68e0e-723">Problem</span><span class="sxs-lookup"><span data-stu-id="68e0e-723">Issue</span></span>
<span data-ttu-id="68e0e-724">Hej AzureEnhancedMonitoring Windows-tjänsten finns och är aktiverad, men misslyckas toostart.</span><span class="sxs-lookup"><span data-stu-id="68e0e-724">hello AzureEnhancedMonitoring Windows service exists and is enabled, but fails toostart.</span></span> <span data-ttu-id="68e0e-725">Mer information finns i hello programmets händelselogg.</span><span class="sxs-lookup"><span data-stu-id="68e0e-725">For more information, check hello application event log.</span></span>

###### <a name="solution"></a><span data-ttu-id="68e0e-726">Lösning</span><span class="sxs-lookup"><span data-stu-id="68e0e-726">Solution</span></span>
<span data-ttu-id="68e0e-727">hello-konfigurationen är felaktig.</span><span class="sxs-lookup"><span data-stu-id="68e0e-727">hello configuration is incorrect.</span></span> <span data-ttu-id="68e0e-728">Starta om hello övervakning tillägget för hello VM, enligt beskrivningen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="68e0e-728">Restart hello monitoring extension for hello VM, as described in [Configure hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span>

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] <span data-ttu-id="68e0e-730">Vissa Azure prestandaräknare saknas</span><span class="sxs-lookup"><span data-stu-id="68e0e-730">Some Azure performance counters are missing</span></span>
<span data-ttu-id="68e0e-731">Hej AzureEnhancedMonitoring Windows-tjänsten samlar in prestandamått i Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0e-731">hello AzureEnhancedMonitoring Windows service collects performance metrics in Azure.</span></span> <span data-ttu-id="68e0e-732">hello-tjänsten hämtar data från flera källor.</span><span class="sxs-lookup"><span data-stu-id="68e0e-732">hello service gets data from several sources.</span></span> <span data-ttu-id="68e0e-733">Några konfigurationsdata som samlas in lokalt och vissa prestandamått läses från Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="68e0e-733">Some configuration data is collected locally, and some performance metrics are read from Azure Diagnostics.</span></span> <span data-ttu-id="68e0e-734">Räknare för lagring som används från din loggning på hello lagringsnivå för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="68e0e-734">Storage counters are used from your logging on hello storage subscription level.</span></span>

<span data-ttu-id="68e0e-735">Om felsökning med hjälp av SAP-kommentar [1999351] inte hello problemet, kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript.</span><span class="sxs-lookup"><span data-stu-id="68e0e-735">If troubleshooting by using SAP Note [1999351] doesn't resolve hello issue, rerun hello `Set-AzureRmVMAEMExtension` configuration script.</span></span> <span data-ttu-id="68e0e-736">Du kanske toowait en timme eftersom storage analytics eller diagnostik räknare inte kan skapas direkt när de är aktiverade.</span><span class="sxs-lookup"><span data-stu-id="68e0e-736">You might have toowait an hour because storage analytics or diagnostics counters might not be created immediately after they are enabled.</span></span> <span data-ttu-id="68e0e-737">Öppna meddelandet SAP customer support på hello komponenten BC-OP-NT-AZR för Windows eller BC-OP-LNX-AZR för en virtuell Linux-dator om hello problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="68e0e-737">If hello problem persists, open an SAP customer support message on hello component BC-OP-NT-AZR for Windows or BC-OP-LNX-AZR for a Linux virtual machine.</span></span>

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] <span data-ttu-id="68e0e-739">Azure prestandaräknare visas inte alls</span><span class="sxs-lookup"><span data-stu-id="68e0e-739">Azure performance counters do not show up at all</span></span>
<span data-ttu-id="68e0e-740">Prestandamått i Azure samlas in av en daemon.</span><span class="sxs-lookup"><span data-stu-id="68e0e-740">Performance metrics in Azure are collected by a daemon.</span></span> <span data-ttu-id="68e0e-741">Om hello daemon inte körs kan inga prestandamått samlas in.</span><span class="sxs-lookup"><span data-stu-id="68e0e-741">If hello daemon is not running, no performance metrics can be collected.</span></span>

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a><span data-ttu-id="68e0e-742">hello installationskatalog hello Azure förbättrad övervakning tillägget är tom</span><span class="sxs-lookup"><span data-stu-id="68e0e-742">hello installation directory of hello Azure Enhanced Monitoring extension is empty</span></span>

###### <a name="issue"></a><span data-ttu-id="68e0e-743">Problem</span><span class="sxs-lookup"><span data-stu-id="68e0e-743">Issue</span></span>
<span data-ttu-id="68e0e-744">hello directory \\var\\lib\\waagent\\ har inte en underkatalog för hello Azure förbättrad övervakning tillägg.</span><span class="sxs-lookup"><span data-stu-id="68e0e-744">hello directory \\var\\lib\\waagent\\ does not have a subdirectory for hello Azure Enhanced Monitoring extension.</span></span>

###### <a name="solution"></a><span data-ttu-id="68e0e-745">Lösning</span><span class="sxs-lookup"><span data-stu-id="68e0e-745">Solution</span></span>
<span data-ttu-id="68e0e-746">hello-tillägget har inte installerats.</span><span class="sxs-lookup"><span data-stu-id="68e0e-746">hello extension is not installed.</span></span> <span data-ttu-id="68e0e-747">Avgöra om detta är en proxy-problemet (enligt beskrivningen ovan).</span><span class="sxs-lookup"><span data-stu-id="68e0e-747">Determine whether this is a proxy issue (as described earlier).</span></span> <span data-ttu-id="68e0e-748">Du kan behöva toorestart hello dator och/eller kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript.</span><span class="sxs-lookup"><span data-stu-id="68e0e-748">You might need toorestart hello machine and/or rerun hello `Set-AzureRmVMAEMExtension` configuration script.</span></span>

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] <span data-ttu-id="68e0e-750">Vissa Azure prestandaräknare saknas</span><span class="sxs-lookup"><span data-stu-id="68e0e-750">Some Azure performance counters are missing</span></span>
<span data-ttu-id="68e0e-751">Prestandamått i Azure samlas in av en daemon som hämtar data från flera källor.</span><span class="sxs-lookup"><span data-stu-id="68e0e-751">Performance metrics in Azure are collected by a daemon, which gets data from several sources.</span></span> <span data-ttu-id="68e0e-752">Några konfigurationsdata som samlas in lokalt och vissa prestandamått läses från Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="68e0e-752">Some configuration data is collected locally, and some performance metrics are read from Azure Diagnostics.</span></span> <span data-ttu-id="68e0e-753">Räknare för lagring komma från hello loggar i prenumerationen lagring.</span><span class="sxs-lookup"><span data-stu-id="68e0e-753">Storage counters come from hello logs in your storage subscription.</span></span>

<span data-ttu-id="68e0e-754">En fullständig och uppdaterad lista över kända problem finns i SAP-kommentar [1999351], som innehåller ytterligare felsökningsinformation för förbättrad Azure-övervakning för SAP.</span><span class="sxs-lookup"><span data-stu-id="68e0e-754">For a complete and up-to-date list of known issues, see SAP Note [1999351], which has additional troubleshooting information for Enhanced Azure Monitoring for SAP.</span></span>

<span data-ttu-id="68e0e-755">Om felsökning med hjälp av SAP-kommentar [1999351] inte hello problemet, kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript enligt beskrivningen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="68e0e-755">If troubleshooting by using SAP Note [1999351] does not resolve hello issue, rerun hello `Set-AzureRmVMAEMExtension` configuration script as described in [Configure hello Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="68e0e-756">Du kanske toowait för en timme eftersom storage analytics eller diagnostik räknare inte kan skapas direkt när de är aktiverade.</span><span class="sxs-lookup"><span data-stu-id="68e0e-756">You might have toowait for an hour because storage analytics or diagnostics counters might not be created immediately after they are enabled.</span></span> <span data-ttu-id="68e0e-757">Öppna meddelandet SAP customer support på hello komponenten BC-OP-NT-AZR för Windows eller BC-OP-LNX-AZR för en virtuell Linux-dator om hello problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="68e0e-757">If hello problem persists, open an SAP customer support message on hello component BC-OP-NT-AZR for Windows or BC-OP-LNX-AZR for a Linux virtual machine.</span></span>
