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
# <a name="deploy-sap-on-windows-vms-in-azure"></a>Distribuera SAP på virtuella Windows-datorer i Azure
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

Virtuella Azure-datorer är hello lösning för organisationer som behöver resurser för beräkning och lagring, minimalt, och utan långa inköp cykler. Du kan använda Azure virtuella datorer toodeploy klassisk program, liksom SAP NetWeaver-baserade program i Azure. Utöka tillförlitlighet och tillgänglighet utan ytterligare lokala resurser för ett program. Virtuella Azure-datorer stöder korsanslutningar, så du kan integrera Azure virtuella datorer i din organisations lokala domäner, privata moln och SAP system liggande.

I den här artikeln beskriver vi hello steg toodeploy SAP-program på Windows-datorer (VM) i Azure. Den här artikeln bygger på hello informationen i [Azure Virtual Machines planering och implementering för SAP på Windows][planning-guide]. Den kompletterar SAP dokumentationen och SAP anteckningar, som är hello primära resurser för att installera och distribuera program.

## <a name="prerequisites"></a>Krav
Konfigurera en virtuell Azure-dator för SAP programdistribution omfattar flera steg och resurser. Innan du börjar bör du kontrollera att du uppfyller hello krav för att installera program på Windows-datorer i Azure.

### <a name="local-computer"></a>Lokal dator
toomanage Windows eller Linux-datorer som du kan använda ett PowerShell-skript och hello Azure-portalen. För båda verktygen behöver du en lokal dator som kör Windows 7 eller en senare version av Windows. Om du vill toomanage endast virtuella Linux-datorer och du vill att toouse en Linux-dator för den här uppgiften kan du använda Azure CLI.

### <a name="internet-connection"></a>Internet-anslutning
toodownload och kör hello verktyg och skript som krävs för SAP programdistribution, måste du vara ansluten toohello Internet. hello virtuella Azure-datorn som kör hello Azure förbättrad övervakning av tillägget för SAP måste också toohello för åtkomst till Internet. Om hello Azure VM är en del av ett virtuellt Azure-nätverk eller lokal domän, kontrollera att hello proxyinställningarna är inställda, enligt beskrivningen i [konfigurera hello proxy][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Microsoft Azure-prenumeration
Du behöver ett Azure-konto.

### <a name="topology-and-networking"></a>Topologi och nätverk
Du behöver toodefine hello topologi och arkitektur för hello SAP-distribution i Azure:

* Azure storage-konton toobe används
* Virtuellt nätverk där du vill att toodeploy hello SAP-systemet
* Resurs grupp toowhich som du vill ha toodeploy hello SAP-system
* Azure-region där du vill att toodeploy hello SAP-systemet
* SAP-konfiguration (två eller tre skikt)
* VM-storlekar och hello antal ytterligare virtuella hårddiskar (VHD) toobe monterade toohello virtuella datorer
* Konfiguration av SAP korrigering och Transport System (CTS)

Skapa och konfigurera Azure storage-konton eller virtuella Azure-nätverk innan du börjar hello SAP process för programvarudistribution. Information om hur toocreate och konfigurera dessa resurser, se [Azure Virtual Machines planering och implementering för SAP på Windows][planning-guide].

### <a name="sap-sizing"></a>Storlek för SAP
Vet hello följande information för SAP storlek:

* Planerat SAP arbetsbelastning, till exempel med hjälp av verktyget för hello SAP snabbt ange och hello SAP programmets prestanda Standard (SAP) tal
* Nödvändiga resurs och minne processorutnyttjande hello SAP-system
* Krävs för in-/ utdata (I/O)-åtgärder per sekund
* Krävs bandbredd på eventuell kommunikation mellan virtuella datorer i Azure
* Nödvändiga nätverksbandbredden mellan lokala tillgångar och hello Azure distribuerade SAP-system

### <a name="resource-groups"></a>Resursgrupper
I Azure Resource Manager, du kan använda resursen grupper toomanage alla hello-resurser i din Azure-prenumeration. Mer information finns i [översikt över Azure Resource Manager][resource-group-overview].

## <a name="resources"></a>Resurser

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-resurser
När du konfigurerar din SAP-programdistribution måste du hello följande SAP-resurser:

* SAP-kommentar [1928533], som innehåller:
  * Lista över Azure VM-storlekar som stöds för hello distributionen av program
  * Viktiga kapacitetsinformation för Azure VM-storlekar
  * Stöds SAP-programvara och operativsystem (OS) och kombinationer av databasen
  * SAP kernel version som krävs för Windows och Linux i Microsoft Azure

* SAP-kommentar [2015553] listar kraven för stöd för SAP SAP programvarudistributioner i Azure.
* SAP-kommentar [2178632] innehåller detaljerad information om all övervakning mått som rapporterats för SAP i Azure.
* SAP-kommentar [1409604] hello krävs SAP värden Agent-version för Windows i Azure.
* SAP-kommentar [2191498] hello krävs SAP värden Agent-version för Linux i Azure.
* SAP-kommentar [2243692] har licensieringsinformation SAP på Linux i Azure.
* SAP-kommentar [1984787] har allmän information om SUSE Linux Enterprise Server 12.
* SAP-kommentar [2002167] har allmän information om Red Hat Enterprise Linux 7.x.
* SAP-kommentar [1999351] innehåller ytterligare felsökningsinformation för hello Azure förbättrad övervakning av tillägget för SAP.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) har alla nödvändiga SAP anteckningar för Linux.
* SAP-specifika PowerShell-cmdletar som ingår i [Azure PowerShell][azure-ps].
* SAP-specifika Azure CLI-kommandona som ingår i [Azure CLI][azure-cli].

[comment]: <> (Lägg till MSSedusch TODO ARM korrigeringsnivå för SAP Värdagenten i SAP Obs 1409604)

### <a name="microsoft-resources"></a>Microsoft-resurser
Dessa Microsoft-artiklarna omfatta SAP-distributioner i Azure:

* [Azure virtuella datorer planering och implementering för SAP i Windows][planning-guide]
* [Distribution av Azure virtuella datorer för SAP på Windows (den här artikeln)][deployment-guide]
* [Azure virtuella datorer DBMS-distribution för SAP i Windows][dbms-guide]
* [Azure-portalen][azure-portal]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Distributionsscenarier för SAP-program på Azure Virtual Machines
Du har flera alternativ för att distribuera virtuella datorer och associerade diskar i Azure. Det är viktigt toounderstand hello skillnaderna mellan distributionsalternativen, eftersom du kan vidta olika steg tooprepare dina virtuella datorer för distribution baserad på hello-distributionstyp som du väljer.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Distribuera en virtuell dator från hello Azure Marketplace för SAP
Du kan använda en avbildning som tillhandahålls av Microsoft eller av en tredje part i hello Azure Marketplace toodeploy den virtuella datorn. hello Marketplace erbjuder vissa standard operativsystemsavbildningar av Windows Server och olika Linux-distributioner. Du kan också distribuera en avbildning som innehåller databashantering system (DBMS) SKU: er, till exempel Microsoft SQL Server. Mer information om hur du använder bilder med DBMS SKU: er finns [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide].

hello visar följande flödesschema hello SAP-specifika sekvens med stegen för att distribuera en virtuell dator från hello Azure Marketplace:

![Flödesdiagram för distribution av Virtuella datorer för SAP-system med hjälp av en VM-avbildning från hello Azure Marketplace][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-hello-azure-portal"></a>Skapa en virtuell dator med hjälp av hello Azure-portalen
hello enklaste sättet toocreate en ny virtuell dator med en avbildning från hello Azure Marketplace är med hjälp av hello Azure-portalen.

1.  Gå för<https://portal.azure.com/#create>.  Välj i hello Azure portal meny **+ ny**.
2.  Välj **Compute**, och välj sedan hello typ av operativsystem som du vill toodeploy. Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12) eller Red Hat Enterprise Linux 7.2 (RHEL 7.2). hello standardvyn lista visar inte alla operativsystem som stöds. Välj **se alla** för en fullständig lista. Mer information om operativsystem som stöds för SAP programdistribution finns SAP-kommentar [1928533].
3.  Granska villkoren på hello nästa sida.
4.  I hello **Välj en distributionsmodell** väljer **Resource Manager**.
5.  Välj **Skapa**.

hello-guiden hjälper dig att ange hello krävs parametrar toocreate hello virtuell dator, förutom tooall krävs resurser, t.ex. nätverksgränssnitt och storage-konton. Några av dessa parametrar är:

1. **Grunderna**:
  * **Namnet**: hello namnet på hello resurs (hello virtuella namn).
 * **Användarnamn och lösenord** eller **offentliga SSH-nyckeln**: Ange hello användarnamn och lösenord för hello-användare som har skapats under hello etablering. För en virtuell Linux-dator, kan du ange hello offentlig SSH (Secure Shell) nyckel som du använder toosign i toohello datorn.
 * **Prenumerationen**: Välj hello prenumeration som du vill toouse tooprovision hello ny virtuell dator.
 * **Resursgruppen**: hello namnet på resursgruppen hello för hello VM. Du kan ange antingen hello-namnet på en ny grupp eller hello resursnamnet för en resursgrupp som redan finns.
 * **Plats**: där toodeploy hello ny virtuell dator. Om du vill tooconnect hello virtuella tooyour lokalt nätverk, kontrollera att du väljer hello platsen för hello virtuella nätverk som ansluter Azure tooyour lokalt nätverk. Mer information finns i [Microsoft Azure-nätverk] [ planning-guide-microsoft-azure-networking] i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide].
2. **Storlek**:

     En lista över VM-typer som stöds finns i SAP-kommentar [1928533]. Måste du markera hello rätt VM-typ. Om du vill toouse Azure Premium-lagring. Inte alla VM-typer stöder Premium-lagring. Mer information finns i [lagring: Microsoft Azure Storage- och datadiskar] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] och [Azure Premium Storage] [ planning-guide-azure-premium-storage] i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide].

3. **Inställningar för**:
   * **Lagringskontot**: Välj ett befintligt lagringskonto eller skapa en ny. Inte alla lagringstyper av fungerar för SAP-program som körs. Mer information om lagringstyper finns [Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide].
   * **Virtuellt nätverk** och **undernät**: toointegrate hello virtuell dator med ditt intranät, Välj hello virtuellt nätverk som är anslutna tooyour lokalt nätverk.
   * **Offentliga IP-adressen**: Välj hello offentlig IP-adress som du vill toouse eller ange parametrar toocreate en ny offentlig IP-adress. Du kan använda en offentlig IP-adress tooaccess din virtuella dator över hello Internet. Kontrollera att du också skapa en network security group toohelp säker åtkomst tooyour virtuell dator.
   * **Nätverkssäkerhetsgruppen**: Mer information finns i [styr flödet i nätverkstrafiken med nätverkssäkerhetsgrupper][virtual-networks-nsg].
   * **Tillgänglighet**: Välj en tillgänglighetsuppsättning eller ange hello parametrar toocreate en ny tillgänglighet anges. Mer information finns i [Azure tillgänglighetsuppsättningar][planning-guide-3.2.3].
   * **Övervaka**: du kan välja **inaktivera** för övervakning av diagnostik. Den aktiveras automatiskt när du kör hello kommandon tooenable hello Azure förbättrad övervakning tillägg, enligt beskrivningen i [Konfigurera övervakning][deployment-guide-configure-monitoring-scenario-1].

4. **Sammanfattning**:

  Granska dina val och välj sedan **OK**.

Den virtuella datorn distribueras i hello resursgrupp som du har valt.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Skapa en virtuell dator med hjälp av en mall
Du kan skapa en virtuell dator med hjälp av en av hello SAP mallarna publicerats i hello [GitHub-lagringsplatsen för azure-snabbstartsmallar][azure-quickstart-templates-github]. Du kan också manuellt skapa en virtuell dator med hjälp av hello [Azure-portalen][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], eller [Azure CLI ][virtual-machines-linux-tutorial].

* [**Tvålagers-(endast en virtuell dator) konfigurationsmallen** (sap-2-nivå-marketplace-avbildning)][sap-templates-2-tier-marketplace-image]

  toocreate system på två nivåer med bara en virtuell dator kan använda den här mallen.
* [**Trelagers-(flera virtuella datorer) konfigurationsmallen** (sap-3-nivå-marketplace-avbildning)][sap-templates-3-tier-marketplace-image]

  toocreate en trelagers-system med flera virtuella datorer och använda den här mallen.

Ange följande parametrar för hello mallen hello i hello Azure-portalen:

1. **Grunderna**:
  * **Prenumerationen**: hello prenumeration toouse toodeploy hello mallen.
  * **Resursgruppen**: hello resource group toouse toodeploy hello mallen. Du kan skapa en ny resursgrupp eller du kan välja en befintlig resursgrupp i hello prenumeration.
  * **Plats**: där toodeploy hello mallen. Om du har valt en befintlig resursgrupp används hello plats som resursgruppen.

2. **Inställningar för**:
  * **System-ID för SAP**: hello SAP System-ID (SID).
  * **OS-typen**: hello operativsystem som du vill toodeploy, till exempel, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12) eller Red Hat Enterprise Linux 7.2 (RHEL 7.2).

    hello standardvyn lista visar inte alla operativsystem som stöds. Välj **se alla** för en fullständig lista. Mer information om operativsystem som stöds för SAP programdistribution finns SAP-kommentar [1928533].
  * **SAP systemstorlek**: hello storleken på hello SAP-system.

    hello antalet SAP hello nya system ger. Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.
  * **Filsystemets tillgänglighet** (endast tre skikt mall): hello filsystemets tillgänglighet.

    Välj **HA** för en konfiguration som passar för en installation med hög tillgänglighet. Två databasservrar och två servrar för ABAP SAP centrala tjänster (ASCS) skapas.
  * **Lagringstyp** (endast två nivåer mall): hello typ av lagring toouse.

    Vi rekommenderar med Azure Premium-lagring för större system. Mer information om lagringstyper finns i följande källor:
      * [Användning av Azure Premium SSD-lagring för SAP DBMS-instans][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]
      * [Premium-lagring: Högpresterande lagring för arbetsbelastningar på Azure-dator][storage-premium-storage-preview-portal]
      * [Introduktion tooMicrosoft Azure Storage][storage-introduction]
  * **Administratörsanvändarnamnet** och **adminlösenord**: ett användarnamn och lösenord.
    En ny användare skapas för att logga in toohello virtuella datorn.
  * **Ny eller befintlig undernät**: Anger om skapas ett nytt virtuellt nätverk och undernät eller ett befintligt undernät används. Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.
  * **Undernät-ID**: hello-ID för virtuella datorer i hello undernät hello ska ansluta till. Välj hello undernät för virtuellt privat nätverk (VPN) eller Azure ExpressRoute virtuellt nätverk toouse tooconnect hello tooyour lokalt nätverk för virtuella datorer. hello ID ser oftast ut så här: /subscriptions/&lt;prenumerations-id > /resourceGroups/&lt;resursgruppens namn > /providers/Microsoft.Network/virtualNetworks/&lt;virtuella nätverksnamnet > /subnets/&lt;undernätsnamn >

3. **Villkor**:  
    Granska och Godkänn hello juridiska villkor.

4.  Välj **inköp**.

hello Azure VM-agenten distribueras som standard när du använder en bild från hello Azure Marketplace.

#### <a name="configure-proxy-settings"></a>Konfigurera proxyinställningar
Beroende på hur det lokala nätverket är konfigurerat kan behöva du tooset in hello proxy på den virtuella datorn. Om den virtuella datorn är ansluten tooyour lokalt nätverk via VPN eller ExpressRoute, kan hello VM inte vara kan tooaccess hello Internet, och inte kan toodownload hello krävs tillägg eller samla in övervakningsdata. Mer information finns i [konfigurera hello proxy][deployment-guide-configure-proxy].

#### <a name="join-a-domain-windows-only"></a>Anslut till en domän (endast Windows)
Om din Azure-distribution är anslutna tooan lokala Active Directory DNS-instans eller via en Azure-plats-till-plats VPN-anslutning eller ExpressRoute (Detta kallas *mellan lokala* i [planering av virtuella datorer i Azure och implementeringen för SAP på Linux][planning-guide]), det förväntas att hello VM är ansluter till en lokal domän. Mer information om överväganden för den här aktiviteten finns [ansluta till en VM tooan lokal domän (Windows)][deployment-guide-4.3].

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Konfigurera övervakning
toobe att din miljö har stöd för SAP, ställa in hello Azure övervakning tillägget för SAP som beskrivs i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5]. Kontrollera hello krav för SAP övervakning och minsta operativsystemversioner som krävs SAP Kernel och SAP Värdagenten hello resurser som anges i [SAP resurser][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Övervakning av kontrollen
Kontrollera om övervakning fungerar, enligt beskrivningen i [kontroller och felsökning för att ställa in slutpunkt till slutpunkt övervakning][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Efter distributionen steg
När du har skapat hello VM och hello VM distribueras måste du tooinstall hello krävs programvarukomponenter i hello VM. På grund av hello distribution och programvara installationen sekvens i den här typen av distribution av Virtuella datorer, måste hello programvara toobe installerad redan finnas, antingen i Azure, på en annan virtuell dator eller som en disk som kan bifogas. Eller, med hjälp av en scenarion med flera lokaler, i vilken anslutningen toohello lokala tillgångar (Installationsresurser) anges.

När du har distribuerat den virtuella datorn i Azure hello följa samma riktlinjer och verktyg tooinstall hello SAP-program på den virtuella datorn som du skulle ha gjort i en lokal miljö. tooinstall SAP-program på en Azure VM, både SAP och Microsoft rekommenderar att du överför och lagra hello SAP-installationsmediet på Azure virtuella hårddiskar eller att du skapar en virtuell Azure-dator som fungerar som en filserver som har alla hello krävs för SAP-installationsmediet.

[comment]: <> (MSSedusch TODO varför vi behöver toorecommend filhantering, till exempel filserver eller VHD? Skiljer sig som så från lokala?)

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Distribuera en virtuell dator med en anpassad avbildning för SAP
Eftersom olika versioner av ett operativsystem eller DBMS har olika krav, uppfyller hello-avbildningar som du hittar i hello Azure Marketplace inte dina behov. I stället kanske du vill toocreate en virtuell dator med hjälp av en egen OS/DBMS VM-avbildning som du kan distribuera igen senare.
Du kan använda olika steg toocreate en privat avbildning för Linux än toocreate ett för Windows.

- - -
> ![Windows][Logo_Windows] Windows
>
> tooprepare en Windows-avbildning som du kan använda toodeploy flera virtuella datorer, hello Windows-inställningar (t.ex. Windows-SID och värdnamn) måste tas ut eller generaliserad på hello lokalt VM. Du kan använda [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) toodo detta.
>
> ![Linux][Logo_Linux] Linux
>
> tooprepare en Linux-avbildning som du kan använda toodeploy flera virtuella datorer, vissa inställningar för Linux måste vara tas ut eller generaliserad på hello lokala VM. Du kan använda `waagent -deprovision` toodo detta. Mer information finns i [avbilda en Linux-dator som körs på Azure] [ virtual-machines-linux-capture-image] och hello [Azure Linux-agenten användarhandboken][virtual-machines-linux-agent-user-guide-command-line-options].
>
>

- - -
Du kan förbereda och skapa en anpassad avbildning och sedan använda toocreate flera nya virtuella datorer. Detta beskrivs i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide]. Konfigurera databasen innehåll genom att använda SAP programvara etablering Manager tooinstall ett nytt SAP-system (återställer en säkerhetskopia av databasen från en virtuell Hårddisk som är bifogade toohello virtuell dator) eller genom att återställa en säkerhetskopia av databasen direkt från Azure storage, om ditt DBMS stöder den. Mer information finns i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]. Om du redan har installerat en SAP-system på den lokala virtuella datorn (särskilt för tvålagers-system), kan du anpassa hello SAP systeminställningar efter hello distributionen av hello Azure VM hello System Byt namn på sätt som stöds av SAP tillhandahållande av programvara Manager (SAP-kommentar [1619720]). Annars kan installera du hello SAP-program när du har distribuerat hello Azure VM.

hello visar följande flödesschema hello SAP-specifika sekvens med stegen för att distribuera en virtuell dator från en anpassad avbildning:

![Flödesdiagram för distribution av Virtuella datorer för SAP-system med hjälp av en VM-avbildning i privata Marketplace][deployment-guide-figure-300]

#### <a name="create-hello-virtual-machine"></a>Skapa hello virtuell dator
toocreate en distribution med hjälp av en privat operativsystemsavbildning från hello Azure-portalen med någon av följande SAP mallar hello. Dessa mallar har publicerats i hello [GitHub-lagringsplatsen för azure-snabbstartsmallar][azure-quickstart-templates-github]. Du kan också manuellt skapa en virtuell dator med hjälp av [PowerShell][virtual-machines-upload-image-windows-resource-manager].

* [**Tvålagers-(endast en virtuell dator) konfigurationsmallen** (sap-2-nivå-användaren-bild)][sap-templates-2-tier-user-image]

  toocreate system på två nivåer med bara en virtuell dator kan använda den här mallen.
* [**Trelagers-(flera virtuella datorer) konfigurationsmallen** (sap-3-nivå-användaren-bild)][sap-templates-3-tier-user-image]

  toocreate en trelagers-system med flera virtuella datorer eller egna OS-avbildningen använder den här mallen.

Ange följande parametrar för hello mallen hello i hello Azure-portalen:

1. **Grunderna**:
  * **Prenumerationen**: hello prenumeration toouse toodeploy hello mallen.
  * **Resursgruppen**: hello resource group toouse toodeploy hello mallen. Du kan skapa en ny resursgrupp eller välj en befintlig resursgrupp i hello prenumerationen.
  * **Plats**: där toodeploy hello mallen. Om du har valt en befintlig resursgrupp används hello plats som resursgruppen.
2. **Inställningar för**:
  * **System-ID för SAP**: hello SAP System-ID.
  * **OS-typen**: hello typ av operativsystem du vill använda toodeploy (Windows eller Linux).
  * **SAP systemstorlek**: hello storleken på hello SAP-system.

    hello antalet SAP hello nya system ger. Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.
  * **Filsystemets tillgänglighet** (endast tre skikt mall): hello filsystemets tillgänglighet.

    Välj **HA** för en konfiguration som passar för en installation med hög tillgänglighet. Två databasservrar och två servrar för ASCS skapas.
  * **Lagringstyp** (endast två nivåer mall): hello typ av lagring toouse.

    Vi rekommenderar med Azure Premium-lagring för större system. Mer information om lagringstyper finns hello följande resurser:
      * [Användning av Azure Premium SSD-lagring för SAP DBMS-instans][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]
      * [Premium-lagring: Högpresterande lagring för arbetsbelastningar på virtuella Azure-datorn][storage-premium-storage-preview-portal]
      * [Introduktion tooMicrosoft Azure Storage][storage-introduction]
  * **Användaravbildningen VHD-URI**: hello URI för hello privata OS-avbildningen virtuell Hårddisk, till exempel https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **Användaren avbildningens lagringskonto**: hello namnet på hello lagringskonto där hello privata OS-avbildningen lagras, till exempel &lt;accountname > i https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **Administratörsanvändarnamnet** och **adminlösenord**: hello användarnamn och lösenord.

    En ny användare skapas för att logga in toohello virtuella datorn.
  * **Ny eller befintlig undernät**: Anger om ett nytt virtuellt nätverk och undernät har skapats eller ett befintligt undernät används. Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.
  * **Undernät-ID**: hello-ID för virtuella datorer i hello undernät toowhich hello ska ansluta till. Välj hello undernät för din VPN eller ExpressRoute virtuellt nätverk toouse tooconnect hello tooyour lokalt nätverk för virtuella datorer. hello ID ser oftast ut så här:

    /subscriptions/&lt;prenumerations-id > /resourceGroups/&lt;resursgruppens namn > /providers/Microsoft.Network/virtualNetworks/&lt;virtuella nätverksnamnet > /subnets/&lt;undernätsnamn >

3. **Villkor**:  
    Granska och Godkänn hello juridiska villkor.

4.  Välj **inköp**.

#### <a name="install-hello-vm-agent-linux-only"></a>Installera hello VM-agenten (endast Linux)
toouse hello mallar beskrivs i föregående avsnitt hello hello Linux-agenten måste vara installerad i hello användaravbildningen eller hello distributionen misslyckas. Hämta och installera hello VM-agenten i hello användaravbildning som beskrivs i [hämta, installera och aktivera hello Azure VM-agenten][deployment-guide-4.4]. Om du inte använder hello mallar kan installera du också hello senare VM-agenten.

#### <a name="join-a-domain-windows-only"></a>Anslut till en domän (endast Windows)
Om din Azure-distribution är anslutna tooan lokala Active Directory DNS-instans eller via en Azure-plats-till-plats VPN-anslutning eller Azure ExpressRoute (Detta kallas *mellan lokala* i [Azure virtuella datorer planering och implementering för SAP på Linux][planning-guide]), det förväntas att hello VM är ansluter till en lokal domän. Mer information om överväganden för det här steget finns [ansluta till en VM tooan lokal domän (Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Konfigurera proxyinställningar
Beroende på hur det lokala nätverket är konfigurerat kan behöva du tooset in hello proxy på den virtuella datorn. Om den virtuella datorn är ansluten tooyour lokalt nätverk via VPN eller ExpressRoute, kan hello VM inte vara kan tooaccess hello Internet, och inte kan toodownload hello krävs tillägg eller samla in övervakningsdata. Mer information finns i [konfigurera hello proxy][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Konfigurera övervakning
toobe att din miljö har stöd för SAP, ställa in hello Azure övervakning tillägget för SAP som beskrivs i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5]. Kontrollera hello krav för SAP övervakning och minsta operativsystemversioner som krävs SAP Kernel och SAP Värdagenten hello resurser som anges i [SAP resurser][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Övervakning av kontrollen
Kontrollera om övervakning fungerar, enligt beskrivningen i [kontroller och felsökning för att ställa in slutpunkt till slutpunkt övervakning][deployment-guide-troubleshooting-chapter].




### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Flytta en virtuell dator lokalt med hjälp av en ej generaliserad Azure VHD med SAP
I det här scenariot kan du planera toomove ett särskilt SAP-system från en lokal miljö tooAzure. Du kan göra detta genom att överföra hello VHD som har hello OS hello SAP binärfiler och slutligen hello DBMS binärfiler plus hello virtuella hårddiskar med hello data och loggfiler för hello DBMS tooAzure. Till skillnad från hello-scenario som beskrivs i [Scenario 2: distribuera en virtuell dator med en anpassad avbildning för SAP][deployment-guide-3.3], i detta fall kan du behålla hello värdnamn, SAP SID och SAP användarkonton i hello Azure VM, eftersom de var konfigurerad i hello lokala miljö. Du behöver inte toogeneralize hello OS. Det här scenariot gäller oftast toocross lokala scenarier där en del av hello SAP liggande körs lokalt och en del av den körs på Azure.

I det här scenariot hello VM-agenten installeras inte automatiskt under distributionen. Eftersom hello VM-agenten och hello Azure förbättrad övervakning av tillägget för SAP krävs för toorun SAP, måste toodownload, installera och aktivera båda komponenterna manuellt när du skapar hello virtuell dator.

Mer information om hello Azure VM-agenten finns i hello följande resurser.

[comment]: <> (MSSedusch TODO uppdatera Windows länk nedan)

- - -
> ![Windows][Logo_Windows] Windows
>
> <http://blogs.msdn.com/b/wats/Archive/2014/02/17/BgInfo-guest-Agent-Extension-for-Azure-VMS.aspx>
>
> ![Linux][Logo_Linux] Linux
>
> [Användarhandboken för Azure Linux-Agent][virtual-machines-linux-agent-user-guide]
>
>

- - -

hello följande flödesschema visar hello sekvens med steg för att flytta en virtuell dator lokalt med hjälp av en ej generaliserad Azure VHD:

![Flödesdiagram för distribution av Virtuella datorer för SAP-system med hjälp av en virtuell disk][deployment-guide-figure-400]

Förutsatt att hello disken är redan överförts och definieras i Azure (se [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide]), gör hello uppgifter beskrivs i hello bredvid avsnitten.

#### <a name="create-a-virtual-machine"></a>Skapa en virtuell dator
toocreate en distribution med hjälp av en privat OS-disk via hello Azure-portalen använder hello SAP mallen publicerats i hello [GitHub-lagringsplatsen för azure-snabbstartsmallar][azure-quickstart-templates-github]. Du kan också manuellt skapa en virtuell dator med hjälp av PowerShell.

* [**Tvålagers-(endast en virtuell dator) konfigurationsmallen** (sap-2-nivå-användaren-disk)][sap-templates-2-tier-os-disk]

  toocreate system på två nivåer med bara en virtuell dator kan använda den här mallen.

Ange följande parametrar för hello mallen hello i hello Azure-portalen:

1. **Grunderna**:
  * **Prenumerationen**: hello prenumeration toouse toodeploy hello mallen.
  * **Resursgruppen**: hello resource group toouse toodeploy hello mallen. Du kan skapa en ny resursgrupp eller välj en befintlig resursgrupp i hello prenumerationen.
  * **Plats**: där toodeploy hello mallen. Om du har valt en befintlig resursgrupp används hello plats som resursgruppen.
2. **Inställningar för**:
  * **System-ID för SAP**: hello SAP System-ID.
  * **OS-typen**: hello typ av operativsystem du vill använda toodeploy (Windows eller Linux).
  * **SAP systemstorlek**: hello storleken på hello SAP-system.

    hello antalet SAP hello nya system ger. Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.
  * **Lagringstyp** (endast två nivåer mall): hello typ av lagring toouse.

    Vi rekommenderar med Azure Premium-lagring för större system. Mer information om lagringstyper finns hello följande resurser:
      * [Användning av Azure Premium SSD-lagring för SAP DBMS-instans][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] i [Azure virtuella DBMS-distribution för SAP på Linux][dbms-guide]
      * [Premium-lagring: Högpresterande lagring för arbetsbelastningar på Azure-dator][storage-premium-storage-preview-portal]
      * [Introduktion tooMicrosoft Azure Storage][storage-introduction]
  * **OS-disken VHD-URI**: hello URI för hello privata OS-disk, till exempel https://&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.
  * **Ny eller befintlig undernät**: Anger om skapas ett nytt virtuellt nätverk och undernät eller ett befintligt undernät används. Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.
  * **Undernät-ID**: hello-ID för virtuella datorer i hello undernät toowhich hello ska ansluta till. Välj hello undernät för din VPN- eller Azure ExpressRoute virtuellt nätverk toouse tooconnect hello tooyour lokalt nätverk för virtuella datorer. hello ID ser oftast ut så här:

    /subscriptions/&lt;prenumerations-id > /resourceGroups/&lt;resursgruppens namn > /providers/Microsoft.Network/virtualNetworks/&lt;virtuella nätverksnamnet > /subnets/&lt;undernätsnamn >

3. **Villkor**:  
    Granska och Godkänn hello juridiska villkor.

4.  Välj **inköp**.

#### <a name="install-hello-vm-agent"></a>Installera hello VM-Agent
toouse hello mallar beskrivs i hello föregående avsnitt, hello VM-agenten måste installeras på hello OS-disk eller hello distributionen misslyckas. Hämta och installera hello VM-agenten i hello VM, enligt beskrivningen i [hämta, installera och aktivera hello Azure VM-agenten][deployment-guide-4.4].

Om du inte använder hello-mallar som beskrivs i föregående avsnitt hello, kan du även installera hello VM-agenten efteråt.

#### <a name="join-a-domain-windows-only"></a>Anslut till en domän (endast Windows)
Om din Azure-distribution är anslutna tooan lokala Active Directory DNS-instans eller via en Azure-plats-till-plats VPN-anslutning eller ExpressRoute (Detta kallas *mellan lokala* i [planering av virtuella datorer i Azure och implementeringen för SAP på Linux][planning-guide]), det förväntas att hello VM är ansluter till en lokal domän. Mer information om överväganden för den här aktiviteten finns [ansluta till en VM tooan lokal domän (Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Konfigurera proxyinställningar
Beroende på hur det lokala nätverket är konfigurerat kan behöva du tooset in hello proxy på den virtuella datorn. Om den virtuella datorn är ansluten tooyour lokalt nätverk via VPN eller ExpressRoute, kan hello VM inte vara kan tooaccess hello Internet, och inte kan toodownload hello krävs tillägg eller samla in övervakningsdata. Mer information finns i [konfigurera hello proxy][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Konfigurera övervakning
toobe att din miljö har stöd för SAP, ställa in hello Azure övervakning tillägget för SAP som beskrivs i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5]. Kontrollera hello krav för SAP övervakning och minsta operativsystemversioner som krävs SAP Kernel och SAP Värdagenten hello resurser som anges i [SAP resurser][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Övervakning av kontrollen
Kontrollera om övervakning fungerar, enligt beskrivningen i [kontroller och felsökning för att ställa in slutpunkt till slutpunkt övervakning][deployment-guide-troubleshooting-chapter].

## <a name="update-hello-monitoring-configuration-for-sap"></a>Uppdatera hello övervakningskonfigurationen för SAP
Uppdatera hello SAP övervakningskonfigurationen i någon av hello följande scenarier:
* hello gemensamma Microsoft/SAP-teamet utökar hello övervakningsfunktionerna och begär fler eller färre räknare.
* Microsoft introducerar en ny version av hello underliggande Azure-infrastrukturen som levererar hello övervakningsdata och hello Azure förbättrad övervakning av tillägget för SAP behov toobe anpassas toothose ändringar.
* Du monterar ytterligare virtuella hårddiskar tooyour Azure VM eller du tar bort en virtuell Hårddisk. Uppdatera hello samling lagringsrelaterade data i det här scenariot. Ändra din konfiguration genom att lägga till eller ta bort slutpunkter eller genom att tilldela IP-påverkar adresser tooa VM inte hello övervakningskonfigurationen.
* Du ändrar hello storleken på den virtuella datorn i Azure, till exempel från storlek A5 tooany andra VM-storlek.
* Du lägger till nya nätverk gränssnitt tooyour Azure VM.

tooupdate övervakning, uppdatering hello övervakning infrastruktur genom att följa hello stegen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].

## <a name="detailed-tasks-for-sap-software-deployment-on-a-windows-vm"></a>Detaljerad information om SAP-programdistribution på en Windows VM
Det här avsnittet innehåller detaljerad steg för specifika uppgifter pågående hello konfiguration och distribution.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Distribuera Azure PowerShell-cmdlets
1.  Gå för[hämtningsbara filer från Microsoft Azure](https://azure.microsoft.com/downloads/).
2.  Under **kommandoradsverktyg**under **PowerShell**väljer **Windows installera**.
3.  Hello Microsoft Download Manager i dialogrutan för hello ned filen (till exempel WindowsAzurePowershellGet.3f.3f.3fnew.exe), Välj **kör**.
4.  Välj toorun Microsoft Web Platform Installer (Microsoft Web PI) **Ja**.
5.  En sida som ser ut som om det här visas:

  ![Installationssidan för Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Välj **installera**, och acceptera licensvillkoren för hello.
7.  PowerShell har installerats. Välj **Slutför** tooclose hello-installationsguiden.

Kontrollera ofta för uppdateringar toohello PowerShell cmdlet: ar, vilket vanligtvis uppdateras varje månad. hello enklaste sättet toocheck för uppdateringar är toodo hello föregående installationssteg upp toohello installation sida som visas i steg 5. hello datum och version versionsnumret hello cmdlets finns på hello-sidan som visas i steg 5. Om inget annat anges i SAP-kommentar [1928533] eller SAP-kommentar [2015553], rekommenderar vi att du arbetar med hello senaste versionen av Azure PowerShell-cmdlets.

toocheck hello version av hello Azure PowerShell-cmdlets som är installerade på datorn, Kör PowerShell-kommando:
```powershell
Import-Module Azure
(Get-Module Azure).Version
```
hello resultatet ser ut så här:

![Resultatet av kontroll av Azure PowerShell-cmdlet-version][deployment-guide-figure-600]
<a name="figure-6"></a>

Om hello Azure cmdlet version är installerad på datorn är hello aktuell version hello första sidan i guiden för installation av hello anger den genom att lägga till **(installerad)** toohello Produkttitel (se följande skärmbild hello). PowerShell Azure-cmdlets är uppdaterade. tooclose hello installationsguiden väljer **avsluta**.

![Installationssidan för Azure PowerShell-cmdlets som anger att hello senaste versionen av Azure PowerShell-cmdlets är installerade][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Distribuera Azure CLI
1.  Gå för[hämtningsbara filer från Microsoft Azure](https://azure.microsoft.com/downloads/).
2.  Under **kommandoradsverktyg**under **Azure-kommandoradsgränssnittet**väljer hello **installera** länk för ditt operativsystem.
3.  Hello Microsoft Download Manager i dialogrutan för hello ned filen (till exempel WindowsAzureXPlatCLI.3f.3f.3fnew.exe), Välj **kör**.
4.  Välj toorun Microsoft Web Platform Installer (Microsoft Web PI) **Ja**.
5.  En sida som ser ut som om det här visas:

  ![Installationssidan för Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Välj **installera**, och acceptera licensvillkoren för hello.
7.  Azure CLI är installerad. Välj **Slutför** tooclose hello-installationsguiden.

Kontrollera ofta för uppdateringar tooAzure CLI, som vanligtvis uppdateras varje månad. hello enklaste sättet toocheck för uppdateringar är toodo hello föregående installationssteg upp toohello installation sida som visas i steg 5.


toocheck hello versionen av Azure CLI som är installerad på datorn som kör det här kommandot:
```
azure --version
```

hello resultatet ser ut så här:

![Resultatet av kontroll av Azure CLI version][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Anslut till en VM tooan lokal domän (endast Windows)
Om du distribuerar SAP virtuella datorer i scenarion med flera lokaler förväntas där lokala Active Directory och DNS har utökats i Azure, att hello virtuella datorer ansluter till en lokal domän. Hej detaljerade steg som du vidta toojoin en VM tooan lokal domän och hello toobe ytterligare programvara som krävs för en medlem av en lokal domän, varierar efter kund. Vanligtvis toojoin en VM-tooan lokala domän måste du tooinstall ytterligare programvara, t.ex. program mot skadlig kod och programvara för säkerhetskopiering och övervakning.

I det här scenariot måste du också toomake till att om Internet-proxyinställningarna tvingas när en virtuell dator ansluter till en domän i din miljö, hello Windows lokala System-kontot (S-1-5-18) i hello gäst VM har hello samma proxyinställningar. hello enklaste alternativet är tooforce hello proxy med hjälp av en grupprincip som gäller toosystems i hello domän-domän.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Hämta, installera och aktivera hello Azure VM-agenten
För virtuella datorer som distribueras från en operativsystemavbildning som inte är generaliserad (till exempel en avbildning som inte kommer från hello Windows-systemförberedelse eller sysprep-verktyget), behöver du toomanually hämta, installera och aktivera hello Azure VM-agenten.

Det här steget är inte nödvändigt om du distribuerar en virtuell dator från hello Azure Marketplace. Bilder från hello Azure Marketplace redan hello Azure VM-agenten.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows
1.  Hämta hello Azure VM-agenten:
  1.  Hämta hello [Azure VM-agenten installationspaketet](https://go.microsoft.com/fwlink/?LinkId=394789).
  2.  Lagra hello VM-agenten MSI-paketet lokalt på en dator eller server.
2.  Installera hello Azure VM-agenten:
  1.  Ansluta toohello distribueras virtuella Azure-datorn med hjälp av Remote Desktop Protocol (RDP).
  2.  Öppna Utforskaren på hello VM och välj hello målkatalogen för hello MSI-filen för hello VM-agenten.
  3.  Dra hello Azure VM-agenten Installer MSI-fil från din lokala dator-servern toohello målkatalogen för hello VM-agenten på hello VM.
  4.  Dubbelklicka på hello MSI-filen på hello VM.
3.  För virtuella datorer som är kopplade tooon lokala domäner, kontrollera att eventuell Internet proxy-inställningar också gäller toohello Windows lokala systemkontot (S-1-5-18) hello VM, enligt beskrivningen i [konfigurera hello proxy] [ deployment-guide-configure-proxy]. hello VM-agenten körs i den här kontexten och måste toobe kan tooconnect tooAzure.

Inga användaråtgärder är obligatoriska tooupdate hello Azure VM-agenten. hello VM-agenten uppdateras automatiskt och kräver inte en VM-omstart.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Använd följande kommandon tooinstall hello VM-Agent för Linux hello:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL)**

  ```
  sudo yum install WALinuxAgent
  ```

Om hello agenten redan är installerad tooupdate hello Azure Linux-agenten hello stegen som beskrivs i [uppdatering hello Azure Linux-agenten på en VM toohello senaste versionen från GitHub][virtual-machines-linux-update-agent].

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Konfigurera hello-proxy
hello steg du utför tooconfigure hello proxy i Windows skiljer sig från hello sättet att konfigurera hello proxy i Linux.

#### <a name="windows"></a>Windows
Proxyinställningar måste ställas in korrekt för hello kontot Lokalt System tooaccess hello Internet. Om proxyinställningarna inte har angetts av en Grupprincip kan du konfigurera hello inställningar för hello kontot Lokalt System.

1. Gå för**starta**, ange **gpedit.msc**, och välj sedan **RETUR**.
2. Välj **Datorkonfiguration** > **Administrationsmallar** > **Windows-komponenter** > **Internet Explorer**. Kontrollera att inställningen hello **Se proxy inställningar per dator (i stället för per användare)** är inaktiverad eller inte konfigurerad.
3. I **Kontrollpanelen**, gå för**nätverks- och delningscenter** > **Internetalternativ**.
4. På hello **anslutningar** fliken, väljer hello **LAN-inställningar** knappen.
5. Rensa hello **automatisk identifiering av inställningar** kryssrutan.
6. Välj hello **använder en proxyserver för nätverket** kryssrutan och sedan ange hello proxyadress och port.
7. Välj hello **Avancerat** knappen.
8. I hello **undantag** ange hello IP-adress **168.63.129.16**. Välj **OK**.


#### <a name="linux"></a>Linux
Konfigurera rätt hello-proxy i hello konfigurationsfil hello Microsoft Azure-Gästagenten, som finns på \\etc\\waagent.conf.

Ange hello följande parametrar:

1.  **HTTP-proxyvärd**. Till exempel den för**proxy.corp.local**.
  ```
  HttpProxy.Host=<proxy host>

  ```
2.  **HTTP-Proxyport**. Till exempel den för**80**.
  ```
  HttpProxy.Port=<port of hello proxy host>

  ```
3.  Starta om hello-agenten.

  ```
  sudo service waagent restart
  ```

Hej proxyinställningarna i \\etc\\waagent.conf gäller även toohello som krävs för VM-tillägg. Om du vill toouse hello Azure databaser, se till att hello trafik toothese databaser inte gå igenom lokalt intranät. Om du har skapat användardefinierade vägar tooenable Tvingad tunneltrafik, se till att du lägger till en väg som dirigerar trafik toohello databaser direkt toohello Internet, och inte via plats-till-plats VPN-anslutningen.

* **SLES**

  Du måste också tooadd vägar för hello IP-adresser som anges i \\etc\\regionserverclnt.cfg. hello visar följande bild ett exempel:

  ![Tvingad tunneltrafik][deployment-guide-figure-50]


* **RHEL**

  Du måste också tooadd vägar för hello IP-adresser för hello värdar som finns i \\etc\\yum.repos.d\\rhui belastningsutjämnare. Ett exempel finns i hello föregående bild.

Mer information om användardefinierade vägar finns [användardefinierade vägar och IP-vidarebefordring][virtual-networks-udr-overview].

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Konfigurera hello Azure förbättrad övervakning av tillägget för SAP
När du har förberett hello VM enligt beskrivningen i [scenarier för distribution av virtuella datorer för SAP på Azure][deployment-guide-3], hello Azure VM-agenten är installerad på hello virtuell dator. hello nästa steg är toodeploy hello Azure förbättrad övervakning av tillägget för SAP, som finns i hello Azure Tilläggscentrallagret i hello globala Azure-datacenter. Mer information finns i [Azure Virtual Machines planering och implementering för SAP på Linux][planning-guide-9.1].

Du kan använda PowerShell eller Azure CLI tooinstall och konfigurera hello Azure förbättrad övervakning av tillägget för SAP. tooinstall hello tillägg på en Windows- eller Linux-VM med hjälp av en Windows-dator finns [Azure PowerShell][deployment-guide-4.5.1]. tooinstall hello tillägg på en Linux-VM med hjälp av en Linux-skrivbord finns [Azure CLI][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell för Linux och Windows-datorer
tooinstall hello Azure förbättrad övervakning av tillägget för SAP med hjälp av PowerShell:

1. Kontrollera att du har installerat hello senaste versionen av hello Azure PowerShell-cmdlet. Mer information finns i [distribution av Azure PowerShell-cmdlets][deployment-guide-4.1].  
2. Kör följande PowerShell-cmdleten hello.
  En lista över tillgängliga miljöer, kör `commandlet Get-AzureRmEnvironment`. Om du vill toouse offentliga Azure måste din miljö är **AzureCloud**. Azure i Kina, Välj **AzureChinaCloud**.


      ```powershell
      $env = Get-AzureRmEnvironment -Name <name of hello environment>
      Login-AzureRmAccount -Environment $env
      Set-AzureRmContext -SubscriptionName <subscription name>

      Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
      ```

När du anger data för ditt konto och identifiera hello Azure-dator, distribuerar hello krävs tillägg hello skript och aktiverar hello krävs funktioner. Det kan ta flera minuter.
Mer information om `Set-AzureRmVMAEMExtension`, se [Set AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].

![SAP-specifika har körts Azure cmdlet Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

Hej `Set-AzureRmVMAEMExtension` konfiguration har steg tooconfigure av alla hello-värden som övervakning för SAP.

utdata för hello skriptet innehåller hello följande information:

* Bekräfta att övervakning för grundläggande hello VHD (med hello OS) och alla ytterligare virtuella hårddiskar monterade toohello VM har konfigurerats.
* följande två hälsningsmeddelande bekräfta hello konfigurationen av Storage-mätvärden för ett visst lagringskonto.
* En rad med utdata ger hello status för hello faktiska uppdateringen av hello övervakningskonfigurationen.
* Ytterligare en rad i utdata bekräftar att hello-konfigurationen har distribuerats eller uppdateras.
* hello sista raden i utdata visas i informationssyfte. Den visar alternativen för tester hello övervakningskonfigurationen.

toocheck att alla steg i Azure-förbättrad övervakning har utförts korrekt och att hello Azure-infrastrukturen tillhandahåller hello nödvändiga data fortsätta med hello beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP, enligt beskrivningen i [Beredskapskontrollen för Azure-förbättrad övervakning för SAP][deployment-guide-5.1]. Vänta 15 30 minuter för Azure-diagnostik toocollect hello relevanta data.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI för virtuella Linux-datorer
tooinstall hello Azure förbättrad övervakning av tillägget för SAP med hjälp av Azure CLI:

1. Installera Azure CLI, enligt beskrivningen i [installera hello Azure CLI][azure-cli].
2. Logga in med ditt Azure-konto:

  ```
  azure login
  ```

3. Växla tooAzure Resource Manager-läge:

  ```
  azure config mode arm
  ```

4. Aktivera Azure utökad övervakning:

  ```
  azure vm enable-aem <resource-group-name> <vm-name>
  ```

5. Kontrollera att hello Azure förbättrad övervakning av tillägget är aktiv på hello virtuella Azure Linux-datorn. Kontrollera om hello filen \\var\\lib\\AzureEnhancedMonitor\\PerfCounters finns. Kör det här kommandot toodisplay information som samlas in av hello Azure förbättrad övervakaren om den finns i en kommandotolk:
```
cat /var/lib/AzureEnhancedMonitor/PerfCounters
```

hello utdata ser ut så här:
```
2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
…
…
```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Kontroller och felsökning för slutpunkt till slutpunkt övervakning
När du har distribuerat den virtuella Azure-datorn och konfigurera hello relevanta Azure övervakade infrastrukturen, kontrollerar du om alla hello komponenter i hello Azure förbättrad övervakning tillägg fungerar som förväntat.

Köra hello beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP enligt beskrivningen i [beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-5.1]. Om alla resultat av beredskap är positivt och alla relevanta prestandaräknare visas OK, Azure övervakning har konfigurerats. Du kan fortsätta med hello installation av SAP Värdagenten som beskrivs i hello SAP anteckningar i [SAP resurser][deployment-guide-2.2]. Om hello beredskapskontroll indikerar att räknare saknas, köra hello hälsokontrollen för hello Azure övervakade infrastrukturen enligt beskrivningen i [hälsokontrollen för Azure infrastructure övervakningskonfigurationen] [ deployment-guide-5.2]. Fler alternativ för felsökning, se [felsökning Azure-övervakning för SAP][deployment-guide-5.3].

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Beredskapskontrollen för hello Azure förbättrad övervakning av tillägget för SAP
Den här kontrollen ser till att alla prestandamått som visas i din SAP-program som tillhandahålls av hello underliggande Azure övervakning av infrastruktur.

#### <a name="run-hello-readiness-check-on-a-windows-vm"></a>Kör hello beredskapskontroll på en Windows VM

1.  Logga in toohello virtuella Azure-datorn (med ett administratörskonto är inte nödvändigt).
2.  Öppna ett kommandotolksfönster.
3.  Ändra hello directory toohello installationsmappen för hello Azure förbättrad övervakning av tillägget för SAP Kommandotolken hello: C:\\paket\\plugin-program\\ Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version >\\släppa

  Hej *version* i hello sökvägen toohello övervakning tillägget kan variera. Om du ser mappar för flera versioner av hello övervakning tillägget hello-installationsmappen, kontrollera hello konfigurationen av hello AzureEnhancedMonitoring Windows-tjänsten och sedan växla toohello mapp anges som *sökvägen tooexecutable* .

  ![Egenskaper för tjänsten som körs hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-figure-1000]

4.  Kommandotolken hello kör **azperflib.exe** utan några parametrar.

  > [!NOTE]
  > Azperflib.exe körs i en slinga och uppdaterar hello samlas in räknare var 60: e sekund. tooend hello slinga, Stäng hello Kommandotolkens fönster.
  >
  >

Om hello Azure förbättrad övervakning av tillägget inte har installerats eller hello AzureEnhancedMonitoring tjänsten inte körs, har hello-tillägget inte konfigurerats korrekt. Detaljerad information om hur toodeploy hello tillägg finns [felsökning hello Azure övervakade infrastrukturen för SAP][deployment-guide-5.3].

##### <a name="check-hello-output-of-azperflibexe"></a>Kontrollera hello utdata från azperflib.exe
Azperflib.exe utdata visar alla fyllts i Azure prestandaräknare för SAP. Längst ned hello hello lista över insamlade räknare visa en sammanfattning och hälsa indikator hello status för övervakning av Azure.

![Utdata för hälsokontrollen genom att köra azperflib.exe som anger att det inte finns några problem][deployment-guide-figure-1100]
<a name="figure-11"></a>

Kontrollera hello resultatet som returneras för hello **räknare totalt** utdata som rapporteras som tom och för **hälsostatus**, visas i hello föregående bild.

Tolka hello värden på följande sätt:

| Azperflib.exe resultatvärden | Azure övervakningsstatusen hälsa |
| --- | --- |
| **API-anrop - inte tillgänglig** | Räknare som inte är tillgänglig kan vara antingen inte tillämpligt toohello virtuella datorkonfigurationen eller är fel. Se **hälsostatus**. |
| **Prestandaräknare för totala – tomt** |hello följande två Azure storage-räknare kan vara tom: <ul><li>Op svarstid för läsning Server msek</li><li>Op svarstid för läsning E2E msek</li></ul>Alla räknare måste ha värden. |
| **Hälsotillstånd** |Endast OK om returnerar status visas **OK**. |
| **Diagnostik** |Detaljerad information om hälsostatus. |

Om hello **hälsostatus** värdet är inte **OK**, följer du anvisningarna hello i [hälsokontrollen för Azure infrastructure övervakningskonfigurationen] [ deployment-guide-5.2].

#### <a name="run-hello-readiness-check-on-a-linux-vm"></a>Kör hello beredskapskontroll på en Linux-VM

1.  Ansluta toohello Azure-dator med hjälp av SSH.

2.  Kontrollera hello utdata från hello Azure förbättrad övervakning av tillägget.

  a.  Kör `more /var/lib/AzureEnhancedMonitor/PerfCounters`

   **Förväntat resultat**: returnerar lista över prestandaräknare. hello-filen får inte vara tomt.

 b. Kör `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error`

   **Förväntat resultat**: returnerar en rad där hello fel är **ingen**, till exempel **3; config; Fel; 0; 0; Ingen; 1456416792; 0 tst-servercs;**

  c. Kör `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord`

    **Förväntat resultat**: returnerar som tom eller finns inte.

Om hello föregående Kontrollera inte lyckades kan du köra dessa ytterligare kontroller:

1.  Kontrollera att hello waagent är installerat och aktiverat.

  a.  Kör `sudo ls -al /var/lib/waagent/`

      **Förväntat resultat**: visas hello innehåll för hello waagent katalog.

  b.  Kör `ps -ax | grep waagent`

   **Förväntat resultat**: Visar en post som liknar:`python /usr/sbin/waagent -daemon`

2. Se till att hello Linux diagnostiska tillägget är installerat och aktiverat.

  a.  Kör `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-'`

   **Förväntat resultat**: visas hello innehåll för hello Linux diagnostiska tillägget katalog.

 b. Kör `ps -ax | grep diagnostic`

   **Förväntat resultat**: Visar en post som liknar:`python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py -daemon`

3.   Kontrollera att hello Azure förbättrad övervakning av tillägget är installerad och körs.

  a.  Kör `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-/'`

    **Förväntat resultat**: Visar hello innehållet i katalog för hello Azure förbättrad övervakning av tillägget.

  b. Kör `ps -ax | grep AzureEnhanced`

     **Förväntat resultat**: Visar en post som liknar:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

3. Installera SAP Värdagenten enligt beskrivningen i SAP-kommentar [1031096], och kontrollera hello utdata från `saposcol`.

  a.  Kör `/usr/sap/hostctrl/exe/saposcol -d`

  b.  Kör `dump ccm`

  c.  Kontrollera om hello **Virtualization_Configuration\Enhanced övervakning åtkomst** mått är **SANT**.

Om du redan har en programserver för SAP NetWeaver ABAP installerat öppna transaktionen ST06 och kontrollera om utökad övervakning är aktiverad.

Om någon av de här kontrollerna misslyckas och detaljerad information om hur tooredeploy hello tillägget ser [felsökning hello Azure övervakade infrastrukturen för SAP][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Hälsotillstånd söka efter hello Azure övervakningskonfigurationen infrastruktur
Om några av hello övervakningsdata inte levereras korrekt enligt hello test som beskrivs i [beredskapskontrollen för Azure-förbättrad övervakning för SAP][deployment-guide-5.1]kör hello `Test-AzureRmVMAEMExtension` cmdlet toocheck Om hello Azure övervakning av infrastruktur och hello övervakning tillägget för SAP är korrekt konfigurerade.

1.  Kontrollera att du har installerat hello senaste versionen av hello Azure PowerShell-cmdleten, enligt beskrivningen i [distribution av Azure PowerShell-cmdlets][deployment-guide-4.1].
2.  Kör följande PowerShell-cmdleten hello. En lista över tillgängliga miljöer kör hello cmdlet `Get-AzureRmEnvironment`. toouse offentlig Azure, Välj hello **AzureCloud** miljö. Azure i Kina, Välj **AzureChinaCloud**.
  ```powershell
  $env = Get-AzureRmEnvironment -Name <name of hello environment>
  Login-AzureRmAccount -Environment $env
  Set-AzureRmContext -SubscriptionName <subscription name>
  Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
  ```

3.  Ange kontodata för och identifiera hello virtuella Azure-datorn.

  ![Inkommande sida i SAP-specifika Azure cmdleten Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

4. hello skript tester hello konfigurationen av hello virtuella dator som du väljer.

  ![Utdata från lyckad test av hello Azure övervakade infrastrukturen för SAP][deployment-guide-figure-1300]

Se till att varje hälsotillstånd resultat är **OK**. Om vissa kontroller inte visas **OK**, köra hello uppdatera cmdlet enligt beskrivningen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5]. Vänta 15 minuter och upprepa hello kontroller som beskrivs i [beredskapskontrollen för Azure-förbättrad övervakning för SAP] [ deployment-guide-5.1] och [hälsokontrollen för Azure-infrastrukturen konfigurationen av övervakning] [deployment-guide-5.2]. Om hello kontrollerar fortfarande tyda på ett problem med vissa eller alla räknare, se [felsökning hello Azure övervakade infrastrukturen för SAP][deployment-guide-5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Felsöka hello Azure övervakade infrastrukturen för SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure prestandaräknare visas inte alls
Hej AzureEnhancedMonitoring Windows-tjänsten samlar in prestandamått i Azure. Om hello-tjänsten inte har installerats på rätt sätt eller om den inte körs i den virtuella datorn, kan inga prestandamått samlas in.

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a>hello installationskatalog hello Azure förbättrad övervakning av tillägget är tom

###### <a name="issue"></a>Problem
hello-installationskatalogen C:\\paket\\plugin-program\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version >\\släpp är tom.

###### <a name="solution"></a>Lösning
hello-tillägget har inte installerats. Avgöra om detta är en proxy-problemet (enligt beskrivningen ovan). Du kan behöva toorestart hello datorn eller kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Tjänsten för Azure-förbättrad övervakning finns inte

###### <a name="issue"></a>Problem
Hej AzureEnhancedMonitoring Windows-tjänsten finns inte.

Azperflib.exe utdata genererar ett fel:

![Körningen av azperflib.exe anger att hello tjänst av hello Azure förbättrad övervakning av tillägget för SAP inte körs][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Lösning
Om hello-tjänsten inte finns, har hello Azure förbättrad övervakning av tillägget för SAP inte installerats korrekt. Distribuera om hello-tillägget med hjälp av stegen för distributionsscenariot i hello [scenarier för distribution av virtuella datorer för SAP i Azure][deployment-guide-3].

När du har distribuerat hello-tillägg, efter en timme, Kontrollera igen om hello Azure prestandaräknare finns i hello Azure VM.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-toostart"></a>Tjänsten för Azure-förbättrad övervakning finns, men inte toostart

###### <a name="issue"></a>Problem
Hej AzureEnhancedMonitoring Windows-tjänsten finns och är aktiverad, men misslyckas toostart. Mer information finns i hello programmets händelselogg.

###### <a name="solution"></a>Lösning
hello-konfigurationen är felaktig. Starta om hello övervakning tillägget för hello VM, enligt beskrivningen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Vissa Azure prestandaräknare saknas
Hej AzureEnhancedMonitoring Windows-tjänsten samlar in prestandamått i Azure. hello-tjänsten hämtar data från flera källor. Några konfigurationsdata som samlas in lokalt och vissa prestandamått läses från Azure-diagnostik. Räknare för lagring som används från din loggning på hello lagringsnivå för prenumerationen.

Om felsökning med hjälp av SAP-kommentar [1999351] inte hello problemet, kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript. Du kanske toowait en timme eftersom storage analytics eller diagnostik räknare inte kan skapas direkt när de är aktiverade. Öppna meddelandet SAP customer support på hello komponenten BC-OP-NT-AZR för Windows eller BC-OP-LNX-AZR för en virtuell Linux-dator om hello problemet kvarstår.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure prestandaräknare visas inte alls
Prestandamått i Azure samlas in av en daemon. Om hello daemon inte körs kan inga prestandamått samlas in.

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a>hello installationskatalog hello Azure förbättrad övervakning tillägget är tom

###### <a name="issue"></a>Problem
hello directory \\var\\lib\\waagent\\ har inte en underkatalog för hello Azure förbättrad övervakning tillägg.

###### <a name="solution"></a>Lösning
hello-tillägget har inte installerats. Avgöra om detta är en proxy-problemet (enligt beskrivningen ovan). Du kan behöva toorestart hello dator och/eller kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript.

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Vissa Azure prestandaräknare saknas
Prestandamått i Azure samlas in av en daemon som hämtar data från flera källor. Några konfigurationsdata som samlas in lokalt och vissa prestandamått läses från Azure-diagnostik. Räknare för lagring komma från hello loggar i prenumerationen lagring.

En fullständig och uppdaterad lista över kända problem finns i SAP-kommentar [1999351], som innehåller ytterligare felsökningsinformation för förbättrad Azure-övervakning för SAP.

Om felsökning med hjälp av SAP-kommentar [1999351] inte hello problemet, kör hello `Set-AzureRmVMAEMExtension` konfigurationsskript enligt beskrivningen i [konfigurera hello Azure förbättrad övervakning av tillägget för SAP][deployment-guide-4.5]. Du kanske toowait för en timme eftersom storage analytics eller diagnostik räknare inte kan skapas direkt när de är aktiverade. Öppna meddelandet SAP customer support på hello komponenten BC-OP-NT-AZR för Windows eller BC-OP-LNX-AZR för en virtuell Linux-dator om hello problemet kvarstår.
