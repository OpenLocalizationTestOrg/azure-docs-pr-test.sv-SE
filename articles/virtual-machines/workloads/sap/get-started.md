---
title: "aaaGetting igång med SAP på Azure Virtual Machines | Microsoft Docs"
description: "Lär dig mer om SAP-lösningar som körs på virtuella datorer (VM) i Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0ac390f8e1c802505b8f9304a12868364fa60f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a><span data-ttu-id="b0daa-103">Med hjälp av Azure för värd för och köra SAP arbetsbelastningsscenarier</span><span class="sxs-lookup"><span data-stu-id="b0daa-103">Using Azure for hosting and running SAP workload scenarios</span></span>
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

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription

[dbms-guide]:dbms-guide.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md 
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab 
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e 
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 
[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b 
[deploy-template-cli]:../../../resource-group-template-deploy.md
[deploy-template-portal]:../../../resource-group-template-deploy.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c


[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056
[ha-guide]:sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md 
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff 
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f 
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a 
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c 
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef 
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a 
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d 
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../../storage/common/storage-premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:../../linux/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

<span data-ttu-id="b0daa-104">Genom att välja Microsoft Azure som partnern SAP redo molnet är kan tooreliably din verksamhetskritiska SAP arbetsbelastningar och scenarier körs på en skalbar, kompatibla och enterprise-beprövade plattform.</span><span class="sxs-lookup"><span data-stu-id="b0daa-104">By choosing Microsoft Azure as your SAP ready cloud partner, you are able tooreliably run your mission critical SAP workloads and scenarios on a scalable, compliant, and enterprise-proven platform.</span></span>  <span data-ttu-id="b0daa-105">Hämta hello skalbarhet, flexibilitet och besparingar i Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-105">Get hello scalability, flexibility, and cost savings of Azure.</span></span> <span data-ttu-id="b0daa-106">Med hello expanderas partnerskap mellan Microsoft och SAP, du kan köra SAP-program över scenarier för utveckling och testning och produktion i Azure - och har fullt stöd.</span><span class="sxs-lookup"><span data-stu-id="b0daa-106">With hello expanded partnership between Microsoft and SAP, you can run SAP applications across dev/test and production scenarios in Azure - and be fully supported.</span></span> <span data-ttu-id="b0daa-107">Vi har du omfattas från SAP NetWeaver tooSAP S4/HANA, SAP BI Linux tooWindows, SAP HANA tooSQL.</span><span class="sxs-lookup"><span data-stu-id="b0daa-107">From SAP NetWeaver tooSAP S4/HANA, SAP BI, Linux tooWindows, SAP HANA tooSQL, we have you covered.</span></span> 

<span data-ttu-id="b0daa-108">Förutom värdscenario SAP NetWeaver med Hej olika DBMS på Azure, du kan vara värd för olika andra scenarier för SAP arbetsbelastning, som SAP BI i Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-108">Besides hosting SAP NetWeaver scenarios with hello different DBMS on Azure, you can host different other SAP workload scenarios, like SAP BI on Azure.</span></span> <span data-ttu-id="b0daa-109">Dokumentation om SAP NetWeaver distributioner på Azure interna virtuella datorer finns i hello avsnittet ”SAP NetWeaver på Azure Virtual Machines”.</span><span class="sxs-lookup"><span data-stu-id="b0daa-109">Documentation regarding SAP NetWeaver deployments on Azure native Virtual Machines can be found in hello section "SAP NetWeaver on Azure Virtual Machines."</span></span> 

<span data-ttu-id="b0daa-110">Azure har inbyggda virtuella Azure-erbjudanden som någonsin växer i storlek på processor och minne resurser toocover SAP arbetsbelastning som utnyttjar SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="b0daa-110">Azure has native Azure Virtual Machine offers that are ever growing in size of CPU and memory resources toocover SAP workload that leverages SAP HANA.</span></span> <span data-ttu-id="b0daa-111">Mer information om det här avsnittet, leta upp hello dokument under hello avsnittet SAP HANA på Azure Virtual Machines ”.</span><span class="sxs-lookup"><span data-stu-id="b0daa-111">For more information on this topic, look up hello documents under hello section SAP HANA on Azure Virtual Machines."</span></span>

<span data-ttu-id="b0daa-112">hello unikhet Azure för SAP HANA är ett unikt erbjudande som anger Azure förutom konkurrens.</span><span class="sxs-lookup"><span data-stu-id="b0daa-112">hello uniqueness of Azure for SAP HANA is a unique offer that sets Azure apart from competition.</span></span> <span data-ttu-id="b0daa-113">I ordning tooenable värd mer minne och processoranvändning dedikerad krävande SAP scenarier som involverar SAP HANA Azure erbjuder hello användning av kunden bare metal-maskinvara för hello syftet med SAP HANA-distributioner som kräver in too20 TB (60 TB skalbar) minne för S/4HANA eller andra SAP HANA-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="b0daa-113">In order tooenable hosting more memory and CPU resource demanding SAP scenarios involving SAP HANA, Azure offers hello usage of customer dedicated bare-metal hardware for hello purpose of running SAP HANA deployments that require up too20 TB (60 TB scale-out) of memory for S/4HANA or other SAP HANA workload.</span></span> <span data-ttu-id="b0daa-114">Det här unika Azure lösningen för SAP HANA i Azure (stora instanser) kan du toorun SAP HANA på hello dedikerad bare metal-maskinvara med hello SAP programnivå eller arbetsbelastning i mitten-ware lager i interna virtuella datorer i Azure som värd.</span><span class="sxs-lookup"><span data-stu-id="b0daa-114">This unique Azure solution of SAP HANA on Azure (Large Instances) allows you toorun SAP HANA on hello dedicated bare-metal hardware with hello SAP application layer or workload middle-ware layer hosted in native Azure Virtual Machines.</span></span> <span data-ttu-id="b0daa-115">Den här lösningen dokumenteras i flera dokument i hello avsnittet ”SAP HANA i Azure (stora instanser)”.</span><span class="sxs-lookup"><span data-stu-id="b0daa-115">This solution is documented in several documents in hello section "SAP HANA on Azure (Large Instances)."</span></span>   

<span data-ttu-id="b0daa-116">Värdscenario SAP arbetsbelastning i Azure kan också skapa kraven i Identity integration och enkel inloggning med hjälp av Azure aktivitet Directory toodifferent SAP-komponenter och SAP SaaS eller PaaS erbjuder.</span><span class="sxs-lookup"><span data-stu-id="b0daa-116">Hosting SAP workload scenarios in Azure also can create requirements of Identity integration and Single-Sign-On using Azure Activity Directory toodifferent SAP components and SAP SaaS or PaaS offers.</span></span> <span data-ttu-id="b0daa-117">En lista över sådana integrering och Single-Sign-On-scenarier med Azure Active Directory (AAD) och SAP entiteter beskrivs och dokumenteras i hello avsnittet ”AAD SAP Identity Integration och Single-Sign-On”.</span><span class="sxs-lookup"><span data-stu-id="b0daa-117">A list of such integration and Single-Sign-On scenarios with Azure Active Directory (AAD) and SAP entities is described and documented in hello section "AAD SAP Identity Integration and Single-Sign-On."</span></span>


## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a><span data-ttu-id="b0daa-118">SAP HANA på SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-118">SAP HANA on SAP HANA on Azure (Large Instances)</span></span>

### <a name="overview-and-architecture-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="b0daa-119">Översikt över och arkitektur för SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-119">Overview and architecture of SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="b0daa-120">Rubrik: aaaOverview och arkitektur för SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-120">title: aaaOverview and Architecture of SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="b0daa-121">Sammanfattning: Den här arkitekturen och tekniska distributionsguiden innehåller information toohelp som du distribuerar SAP på hello nya SAP HANA i Azure (stora instanser) i Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-121">Summary: This Architecture and Technical Deployment Guide provides information toohelp you deploy SAP on hello new SAP HANA on Azure (Large Instances) in Azure.</span></span> <span data-ttu-id="b0daa-122">Det är inte avsedda toobe en heltäckande handbok omfattar specifika inställningarna för SAP-lösningar, men i stället användbar information i ditt första distributionen och pågående drift.</span><span class="sxs-lookup"><span data-stu-id="b0daa-122">It is not intended toobe a comprehensive guide covering specific setup of SAP solutions, but rather useful information in your initial deployment and ongoing operations.</span></span> <span data-ttu-id="b0daa-123">Det bör inte ersätta SAP dokumentationen relaterade toohello installation av SAP HANA (eller hello många SAP stöd anteckningar som täcker hello-avsnittet).</span><span class="sxs-lookup"><span data-stu-id="b0daa-123">It should not replace SAP documentation related toohello installation of SAP HANA (or hello many SAP Support Notes that cover hello topic).</span></span> <span data-ttu-id="b0daa-124">Den ger dig en översikt och ger hello ytterligare information om att installera SAP HANA i Azure (stora instanser).</span><span class="sxs-lookup"><span data-stu-id="b0daa-124">It gives you an overview and provides hello additional detail of installing SAP HANA on Azure (Large Instances).</span></span>

<span data-ttu-id="b0daa-125">Uppdaterad: Juli 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-125">Updated: July 2017</span></span>

[<span data-ttu-id="b0daa-126">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-126">This guide can be found here</span></span>](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="infrastructure-and-connectivity-toosap-hana-on-azure-large-instances"></a><span data-ttu-id="b0daa-127">Infrastruktur och anslutningen tooSAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-127">Infrastructure and connectivity tooSAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="b0daa-128">Rubrik: aaaInfrastructure och anslutningen tooSAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-128">title: aaaInfrastructure and Connectivity tooSAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="b0daa-129">Sammanfattning: När hello inköp av SAP HANA i Azure (stora instanser) är slutförd mellan dig och hello Microsoft enterprise-kontoteamet krävs för olika nätverkskonfigurationer i rätt ordning tooensure-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="b0daa-129">Summary: After hello purchase of SAP HANA on Azure (Large Instances) is finalized between you and hello Microsoft enterprise account team, various network configurations are required in order tooensure proper connectivity.</span></span>  <span data-ttu-id="b0daa-130">Det här dokumentet beskrivs hello information som har toobe som delas med hello följande information krävs.</span><span class="sxs-lookup"><span data-stu-id="b0daa-130">This document outlines hello information that has toobe shared with hello following information is required.</span></span> <span data-ttu-id="b0daa-131">Det här dokumentet beskrivs vilken information som har samlats in toobe och vilka konfigurationsskript har toobe kör.</span><span class="sxs-lookup"><span data-stu-id="b0daa-131">This document outlines what information has toobe collected and what configuration scripts have toobe run.</span></span> 

<span data-ttu-id="b0daa-132">Uppdaterad: Juli 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-132">Updated: July 2017</span></span>

[<span data-ttu-id="b0daa-133">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-133">This guide can be found here</span></span>](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="install-sap-hana-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="b0daa-134">Installera SAP HANA i SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-134">Install SAP HANA in SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="b0daa-135">Rubrik: aaaInstall SAP HANA på SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-135">title: aaaInstall SAP HANA on SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="b0daa-136">Sammanfattning: Det här dokumentet beskrivs hello anvisningar för att installera SAP HANA på din stora Azure-instans.</span><span class="sxs-lookup"><span data-stu-id="b0daa-136">Summary: This document outlines hello setup procedures for installing SAP HANA on your Azure Large Instance.</span></span> 

<span data-ttu-id="b0daa-137">Uppdaterad: Juli 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-137">Updated: July 2017</span></span>

[<span data-ttu-id="b0daa-138">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-138">This guide can be found here</span></span>](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="high-availability-and-disaster-recovery-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="b0daa-139">Hög tillgänglighet och katastrofåterställning återställning av SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-139">High availability and disaster recovery of SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="b0daa-140">Rubrik: aaaHigh tillgänglighet och Disaster Recovery för SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-140">title: aaaHigh Availability and Disaster Recovery of SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="b0daa-141">Sammanfattning: Hög tillgänglighet och Disaster Recovery (DR) är mycket viktiga aspekter av din verksamhetskritiska SAP HANA körs på Azure (stora instanser) servrar.</span><span class="sxs-lookup"><span data-stu-id="b0daa-141">Summary: High Availability (HA) and Disaster Recovery (DR) are very important aspects of running your mission-critical SAP HANA on Azure (Large Instances) server(s).</span></span> <span data-ttu-id="b0daa-142">Dess import toowork med SAP, din systemintegreraren och/eller Microsoft tooproperly skapa och implementera hello höger hr/DR-strategi för dig.</span><span class="sxs-lookup"><span data-stu-id="b0daa-142">It's import toowork with SAP, your system integrator, and/or Microsoft tooproperly architect and implement hello right HA/DR strategy for you.</span></span> <span data-ttu-id="b0daa-143">Viktiga saker som mål för återställning-plats (RPO) och återställning tid mål för Återställningstid, specifika tooyour miljö måste beaktas.</span><span class="sxs-lookup"><span data-stu-id="b0daa-143">Important considerations like Recovery Point Objective (RPO) and Recovery Time Objective (RTO), specific tooyour environment, must be considered.</span></span>  <span data-ttu-id="b0daa-144">Det här dokumentet beskriver alternativen för att aktivera din önskade nivå med hög tillgänglighet och Katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="b0daa-144">This document explains your options for enabling your preferred level of HA and DR.</span></span>

<span data-ttu-id="b0daa-145">Uppdaterad: December 2016</span><span class="sxs-lookup"><span data-stu-id="b0daa-145">Updated: December 2016</span></span>

[<span data-ttu-id="b0daa-146">Det här dokumentet finns här</span><span class="sxs-lookup"><span data-stu-id="b0daa-146">This document can be found here</span></span>](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="troubleshooting-and-monitoring-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="b0daa-147">Felsökning och övervakning av SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-147">Troubleshooting and monitoring of SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="b0daa-148">Rubrik: aaaTroubleshooting och övervakning av SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="b0daa-148">title: aaaTroubleshooting and Monitoring of SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="b0daa-149">Sammanfattning: Den här handboken innehåller information som är användbara vid upprättandet av övervakning av din SAP HANA på Azure-miljön samt ytterligare information om felsökning.</span><span class="sxs-lookup"><span data-stu-id="b0daa-149">Summary: This guide covers information that is useful in establishing monitoring of your SAP HANA on Azure environment as well as additional troubleshooting information.</span></span> 

<span data-ttu-id="b0daa-150">Uppdaterad: December 2016</span><span class="sxs-lookup"><span data-stu-id="b0daa-150">Updated: December 2016</span></span>

[<span data-ttu-id="b0daa-151">Det här dokumentet finns här</span><span class="sxs-lookup"><span data-stu-id="b0daa-151">This document can be found here</span></span>](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="sap-hana-on-azure-virtual-machines"></a><span data-ttu-id="b0daa-152">SAP HANA på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b0daa-152">SAP HANA on Azure Virtual Machines</span></span>

### <a name="getting-started-with-sap-hana-on-azure"></a><span data-ttu-id="b0daa-153">Komma igång med SAP HANA på Azure</span><span class="sxs-lookup"><span data-stu-id="b0daa-153">Getting started with SAP HANA on Azure</span></span>
<span data-ttu-id="b0daa-154">Rubrik: aaaQuickstart guide för manuell installation av SAP HANA på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b0daa-154">title: aaaQuickstart guide for manual installation of SAP HANA on Azure VMs</span></span>

<span data-ttu-id="b0daa-155">Sammanfattning: Den här snabbstartsguide hjälper tooset single instance SAP HANA system på virtuella Azure-datorer med en manuell installation av SAP NetWeaver 7.5 och SAP HANA SP12.</span><span class="sxs-lookup"><span data-stu-id="b0daa-155">Summary: This quickstart guide helps tooset up a single-instance SAP HANA system on Azure VMs by a manual installation of SAP NetWeaver 7.5 and SAP HANA SP12.</span></span> <span data-ttu-id="b0daa-156">hello handboken förutsätter att hello läsaren är bekant med grundläggande Azure IaaS som hur toodeploy virtuella datorer eller virtuella nätverk, antingen via hello Azure-portalen eller Powershell/CLI inklusive hello alternativet toouse json-mallarna.</span><span class="sxs-lookup"><span data-stu-id="b0daa-156">hello guide presumes that hello reader is familiar with Azure IaaS basics like how toodeploy virtual machines or virtual networks either via hello Azure portal or Powershell/CLI including hello option toouse json templates.</span></span> <span data-ttu-id="b0daa-157">Dessutom förväntas att hello läsaren är bekant med SAP HANA, SAP NetWeaver och hur tooinstall det lokalt.</span><span class="sxs-lookup"><span data-stu-id="b0daa-157">Furthermore it's expected that hello reader is familiar with SAP HANA, SAP NetWeaver and how tooinstall it on-premises.</span></span>

<span data-ttu-id="b0daa-158">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-158">Updated: June 2017</span></span>

[<span data-ttu-id="b0daa-159">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-159">This guide can be found here</span></span>](hana-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="s4hana-sap-cal-deployment-on-azure"></a><span data-ttu-id="b0daa-160">S/4HANA CAL SAP-distribution på Azure</span><span class="sxs-lookup"><span data-stu-id="b0daa-160">S/4HANA SAP CAL deployment on Azure</span></span>
<span data-ttu-id="b0daa-161">Rubrik: aaaDeploy SAP S/4HANA eller BW/4HANA på Azure</span><span class="sxs-lookup"><span data-stu-id="b0daa-161">title: aaaDeploy SAP S/4HANA or BW/4HANA on Azure</span></span>

<span data-ttu-id="b0daa-162">Sammanfattning: Den här guiden hjälper toodemonstrate hello distribution av SAP S/4HANA på Azure med hjälp av SAP Molnbibliotek för installation.</span><span class="sxs-lookup"><span data-stu-id="b0daa-162">Summary: This guide helps toodemonstrate hello deployment of SAP S/4HANA on Azure using SAP Cloud Appliance Library.</span></span> <span data-ttu-id="b0daa-163">SAP Molnbibliotek installation är en tjänst av SAP som gör att toodeploy SAP-program på Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-163">SAP Cloud Appliance Library is a service by SAP that allows toodeploy SAP applications on Azure.</span></span> <span data-ttu-id="b0daa-164">hello guiden beskriver steg för steg hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="b0daa-164">hello guide describes step by step hello deployment.</span></span>

<span data-ttu-id="b0daa-165">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-165">Updated: June 2017</span></span>

[<span data-ttu-id="b0daa-166">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-166">This guide can be found here</span></span>](cal-s4h.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="high-availability-of-sap-hana-in-azure-virtual-machines"></a><span data-ttu-id="b0daa-167">Hög tillgänglighet för SAP HANA i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-167">High Availability of SAP HANA in Azure Virtual Machines</span></span>
<span data-ttu-id="b0daa-168">Rubrik: aaaHigh tillgänglighet för SAP HANA på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b0daa-168">title: aaaHigh Availability of SAP HANA on Azure Virtual Machines</span></span>

<span data-ttu-id="b0daa-169">Sammanfattning: Den här guiden leder dig genom hello konfiguration för hög tillgänglighet för hello SUSE 12 OS och SAP HANA tooaccommodate HANA System replikering med automatisk redundans.</span><span class="sxs-lookup"><span data-stu-id="b0daa-169">Summary: This guide leads you through hello high availability configuration of hello SUSE 12 OS and SAP HANA tooaccommodate HANA System replication with automatic failover.</span></span> <span data-ttu-id="b0daa-170">hello guide är specifika för SUSE och virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-170">hello guide is specific for SUSE and Azure Virtual Machines.</span></span> <span data-ttu-id="b0daa-171">hello guiden gäller inte ännu för Red Hat eller bare metal- eller privata moln eller andra Azure-offentligt moln-distributioner.</span><span class="sxs-lookup"><span data-stu-id="b0daa-171">hello guide does not apply yet for Red Hat or bare-metal or private cloud or other non-Azure public cloud deployments.</span></span>

<span data-ttu-id="b0daa-172">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-172">Updated: June 2017</span></span>

[<span data-ttu-id="b0daa-173">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-173">This guide can be found here</span></span>](sap-hana-high-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-backup-overview-on-azure-vms"></a><span data-ttu-id="b0daa-174">Översikt av SAP HANA säkerhetskopiering på virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-174">SAP HANA backup overview on Azure VMs</span></span>
<span data-ttu-id="b0daa-175">Rubrik: aaaBackup guide för SAP HANA på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b0daa-175">title: aaaBackup guide for SAP HANA on Azure Virtual Machines</span></span>

<span data-ttu-id="b0daa-176">Sammanfattning: Den här guiden innehåller grundläggande information om säkerhetskopiering möjligheter som körs på Azure Virtual Machines för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="b0daa-176">Summary: This guide provides basic information about backup possibilities running SAP HANA on Azure Virtual Machines.</span></span>

<span data-ttu-id="b0daa-177">Uppdaterad: Mars 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-177">Updated: March 2017</span></span>

[<span data-ttu-id="b0daa-178">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-178">This guide can be found here</span></span>](sap-hana-backup-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-file-level-backup-on-azure-vms"></a><span data-ttu-id="b0daa-179">SAP HANA nivån säkerhetskopiering på virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-179">SAP HANA file level backup on Azure VMs</span></span>
<span data-ttu-id="b0daa-180">Rubrik: aaaSAP HANA säkerhetskopiering baserat på lagring ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="b0daa-180">title: aaaSAP HANA backup based on storage snapshots</span></span>

<span data-ttu-id="b0daa-181">Sammanfattning: Den här guiden innehåller information om att använda ögonblicksbilder säkerhetskopiering på virtuella Azure-datorer när du kör SAP HANA på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="b0daa-181">Summary: This guide provides information about using snapshot-based backups on Azure VMs when running SAP HANA on Azure Virtual Machines.</span></span>

<span data-ttu-id="b0daa-182">Uppdaterad: Mars 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-182">Updated: March 2017</span></span>

[<span data-ttu-id="b0daa-183">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-183">This guide can be found here</span></span>](sap-hana-backup-file-level.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="sap-hana-snapshot-based-backups-on-azure-vms"></a><span data-ttu-id="b0daa-184">Säkerhetskopior av SAP HANA-ögonblicksbilder baserat på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b0daa-184">SAP HANA snapshot based backups on Azure VMs</span></span>
<span data-ttu-id="b0daa-185">Rubrik: aaaSAP HANA Azure Backup på filnivå</span><span class="sxs-lookup"><span data-stu-id="b0daa-185">title: aaaSAP HANA Azure Backup on file level</span></span>

<span data-ttu-id="b0daa-186">Sammanfattning: Den här guiden innehåller information om hur du använder SAP HANA nivån säkerhetskopiering körs på Azure Virtual Machines för SAP HANA</span><span class="sxs-lookup"><span data-stu-id="b0daa-186">Summary: This guide provides information about using SAP HANA file level backup running SAP HANA on Azure Virtual Machines</span></span>

<span data-ttu-id="b0daa-187">Uppdaterad: Mars 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-187">Updated: March 2017</span></span>

[<span data-ttu-id="b0daa-188">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-188">This guide can be found here</span></span>](sap-hana-backup-storage-snapshots.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a><span data-ttu-id="b0daa-189">SAP NetWeaver distribueras på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b0daa-189">SAP NetWeaver deployed on Azure Virtual Machines</span></span>

### <a name="deploy-sap-ides-system-on-windows-and-sql-server-through-sap-cal-on-azure"></a><span data-ttu-id="b0daa-190">Distribuera SAP IDES system i Windows och SQL Server via SAP CAL på Azure</span><span class="sxs-lookup"><span data-stu-id="b0daa-190">Deploy SAP IDES system on Windows and SQL Server through SAP CAL on Azure</span></span>
<span data-ttu-id="b0daa-191">Rubrik: aaaTesting SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-191">title: aaaTesting SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span> 

<span data-ttu-id="b0daa-192">Sammanfattning: Det här dokumentet beskriver hello distributionen av ett SAP IDES system baserat på Windows och SQL Server på Azure med hjälp av SAP Molnbibliotek för installation.</span><span class="sxs-lookup"><span data-stu-id="b0daa-192">Summary: This document describes hello deployment of an SAP IDES system based on Windows and SQL Server on Azure using SAP Cloud Appliance Library.</span></span> <span data-ttu-id="b0daa-193">SAP molnet installation biblioteket är en SAP-tjänst som gör hello distribution av SAP produkter på Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-193">SAP Cloud appliance Library is an SAP service that allows hello deployment of SAP products on Azure.</span></span> <span data-ttu-id="b0daa-194">Det här dokumentet går steg för steg igenom hello distributionen av ett SAP IDES system.</span><span class="sxs-lookup"><span data-stu-id="b0daa-194">This document goes step by step through hello deployment of an SAP IDES system.</span></span> <span data-ttu-id="b0daa-195">hello IDES system är bara ett exempel för flera andra dussin program som kan distribueras via SAP molnet installation på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-195">hello IDES system is just an example for several other dozen applications that can be deployed through SAP Cloud appliance on Microsoft Azure.</span></span>

<span data-ttu-id="b0daa-196">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-196">Updated: June 2017</span></span>

[<span data-ttu-id="b0daa-197">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-197">This guide can be found here</span></span>](cal-ides-erp6-erp7-sp3-sql.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="quickstart-guide-for-netweaver-on-suse-linux-on-azure"></a><span data-ttu-id="b0daa-198">Snabbstartsguide för NetWeaver på SUSE Linux på Azure</span><span class="sxs-lookup"><span data-stu-id="b0daa-198">Quickstart guide for NetWeaver on SUSE Linux on Azure</span></span>
<span data-ttu-id="b0daa-199">Rubrik: aaaTesting SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-199">title: aaaTesting SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span> 

<span data-ttu-id="b0daa-200">Sammanfattning: Den här artikeln beskrivs olika saker tooconsider när du kör SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="b0daa-200">Summary: This article describes various things tooconsider when you're running SAP NetWeaver on Microsoft Azure SUSE Linux virtual machines (VMs).</span></span> <span data-ttu-id="b0daa-201">SAP NetWeaver stöds officiellt på SUSE Linux virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-201">SAP NetWeaver is officially supported on SUSE Linux VMs on Azure.</span></span> <span data-ttu-id="b0daa-202">All information om Linux-versioner, SAP kernel-versioner och annan information finns i SAP Obs 1928533 ”SAP-program i Azure: produkter som stöds och Azure VM typer”.</span><span class="sxs-lookup"><span data-stu-id="b0daa-202">All details regarding Linux versions, SAP kernel versions, and other details can be found in SAP Note 1928533 "SAP Applications on Azure: Supported Products and Azure VM types".</span></span>

<span data-ttu-id="b0daa-203">Uppdaterad: September 2016</span><span class="sxs-lookup"><span data-stu-id="b0daa-203">Updated: September 2016</span></span>

[<span data-ttu-id="b0daa-204">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-204">This guide can be found here</span></span>](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <span data-ttu-id="b0daa-205"><a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Planering och implementering</span><span class="sxs-lookup"><span data-stu-id="b0daa-205"><a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Planning and implementation</span></span>
<span data-ttu-id="b0daa-206">Rubrik: aaaAzure virtuella datorer planering och implementering för SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="b0daa-206">title: aaaAzure Virtual Machines planning and implementation for SAP NetWeaver</span></span>

<span data-ttu-id="b0daa-207">Sammanfattning: Det här dokumentet är hello guiden toostart med om du funderar SAP NetWeaver i Azure virtuella datorer som körs.</span><span class="sxs-lookup"><span data-stu-id="b0daa-207">Summary: This document is hello guide toostart with if you are thinking about running SAP NetWeaver in Azure Virtual Machines.</span></span> <span data-ttu-id="b0daa-208">Den här guiden för planering och implementering hjälper dig att utvärdera om en befintlig eller planerade SAP NetWeaver-dator kan vara distribuerade tooan virtuella datorer i Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="b0daa-208">This planning and implementation guide helps you evaluate whether an existing or planned SAP NetWeaver-based system can be deployed tooan Azure Virtual Machines environment.</span></span> <span data-ttu-id="b0daa-209">Den omfattar flera SAP NetWeaver distributionsscenarier och innehåller SAP-konfigurationer som är specifika tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-209">It covers multiple SAP NetWeaver deployment scenarios, and includes SAP configurations that are specific tooAzure.</span></span> <span data-ttu-id="b0daa-210">hello papper listas och beskrivs alla hello nödvändig konfigurationsinformation måste på hello SAP-/ Azure sida toorun hybrid SAP liggande.</span><span class="sxs-lookup"><span data-stu-id="b0daa-210">hello paper lists and describes all hello necessary configuration information you’ll need on hello SAP/Azure side toorun a hybrid SAP landscape.</span></span> <span data-ttu-id="b0daa-211">Åtgärder som du kan vidta tooensure hög tillgänglighet för SAP NetWeaver-baserade system på IaaS omfattas också.</span><span class="sxs-lookup"><span data-stu-id="b0daa-211">Measures you can take tooensure high availability of SAP NetWeaver-based systems on IaaS are also covered.</span></span>

<span data-ttu-id="b0daa-212">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-212">Updated: June 2017</span></span>

<span data-ttu-id="b0daa-213">[Den här guiden hittar du här][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="b0daa-213">[This guide can be found here][planning-guide]</span></span>

### <a name="high-availability-configurations-of-sap-netweaver-in-azure-vms"></a><span data-ttu-id="b0daa-214">Hög tillgänglighet konfigurationer för SAP NetWeaver i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-214">High Availability configurations of SAP NetWeaver in Azure VMs</span></span>
<span data-ttu-id="b0daa-215">Rubrik: aaaAzure hög tillgänglighet för virtuella datorer för SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="b0daa-215">title: aaaAzure Virtual Machines high availability for SAP NetWeaver</span></span>

<span data-ttu-id="b0daa-216">Sammanfattning: I det här dokumentet vi upp hello stegen för att toodeploy SAP-system med hög tillgänglighet i Azure med hjälp av hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b0daa-216">Summary: In this document, we cover hello steps that you can take toodeploy high-availability SAP systems in Azure by using hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="b0daa-217">Vi dig igenom dessa viktiga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b0daa-217">We walk you through these major tasks.</span></span> <span data-ttu-id="b0daa-218">I hello dokumentet beskrivs hur en –--felpunkter komponenter som avancerade Business Application Programming (ABAP) SAP centrala tjänster (ASCS) / SAP centrala Services (SCS) och databashanteringssystem (DBMS) och redundanta komponenter som SAP Programservern kommer toobe skyddas när den körs i virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="b0daa-218">In hello document, we describe how single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server are going toobe protected when running in Azure VMs.</span></span> <span data-ttu-id="b0daa-219">Ett steg för steg-exempel på en installation och konfiguration av en SAP-systemet för hög tillgänglighet i ett redundanskluster i Windows Server-kluster i Azure visas och visas i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="b0daa-219">A step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure is demonstrated and shown in this document.</span></span>

<span data-ttu-id="b0daa-220">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-220">Updated: June 2017</span></span>

[<span data-ttu-id="b0daa-221">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-221">This guide can be found here</span></span>](high-availability-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="realizing-multi-sid-deployments-of-sap-netweaver-in-azure-vms"></a><span data-ttu-id="b0daa-222">Inse Multi-SID distributioner av SAP NetWeaver i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-222">Realizing Multi-SID deployments of SAP NetWeaver in Azure VMs</span></span>
<span data-ttu-id="b0daa-223">Rubrik: aaaCreate en SAP NetWeaver multi-SID-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b0daa-223">title: aaaCreate an SAP NetWeaver multi-SID configuration</span></span> 

<span data-ttu-id="b0daa-224">Sammanfattning: Det här dokumentet är tillägg toohello hög tillgänglighet för SAP NetWeaver på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="b0daa-224">Summary: This document is an addition toohello document High availability for SAP NetWeaver on Azure VMs.</span></span> <span data-ttu-id="b0daa-225">På grund av toonew funktioner i Azure som har introducerats i September 2016, är det möjligt toodeploy flera SAP NetWeaver ASCS/SCS förekomster i ett par med virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="b0daa-225">Due toonew functionality in Azure that got introduced in September 2016, it is possible toodeploy multiple SAP NetWeaver ASCS/SCS instances in a pair of Azure VMs.</span></span> <span data-ttu-id="b0daa-226">Med denna konfiguration kan du minska hello antalet virtuella datorer nödvändiga toodeploy toorealize högtillgänglig SAP NetWeaver konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="b0daa-226">With such a configuration, you can reduce hello number of VMs necessary toodeploy toorealize highly available SAP NetWeaver configurations.</span></span> <span data-ttu-id="b0daa-227">hello guiden beskriver hello installationen av konfigurationerna multi-SID.</span><span class="sxs-lookup"><span data-stu-id="b0daa-227">hello guide describes hello setup of such multi-SID configurations.</span></span>

<span data-ttu-id="b0daa-228">Uppdaterad: December 2016</span><span class="sxs-lookup"><span data-stu-id="b0daa-228">Updated: December 2016</span></span>

[<span data-ttu-id="b0daa-229">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-229">This guide can be found here</span></span>](high-availability-multi-sid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <span data-ttu-id="b0daa-230"><a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Distribution av SAP NetWeaver i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0daa-230"><a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Deployment of SAP NetWeaver in Azure VMs</span></span>
<span data-ttu-id="b0daa-231">Rubrik: aaaAzure distribution av virtuella datorer för SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="b0daa-231">title: aaaAzure Virtual Machines deployment for SAP NetWeaver</span></span>

<span data-ttu-id="b0daa-232">Sammanfattning: I detta dokument ger procedurvägledning för distribution SAP NetWeaver programvara toovirtual datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-232">Summary: This document provides procedural guidance for deploying SAP NetWeaver software toovirtual machines in Azure.</span></span> <span data-ttu-id="b0daa-233">Det här dokumentet fokuserar på tre specifika distributionsscenarier med betoning på Aktivera hello Azure övervakning tillägg för SAP, inklusive Felsökningsrekommendationer för hello Azure övervakning tillägg för SAP.</span><span class="sxs-lookup"><span data-stu-id="b0daa-233">This paper focuses on three specific deployment scenarios, with an emphasis on enabling hello Azure Monitoring Extensions for SAP, including troubleshooting recommendations for hello Azure Monitoring Extensions for SAP.</span></span> <span data-ttu-id="b0daa-234">Det här dokumentet förutsätter att du har läst hello planering och implementering guide.</span><span class="sxs-lookup"><span data-stu-id="b0daa-234">This paper assumes that you’ve read hello planning and implementation guide.</span></span>

<span data-ttu-id="b0daa-235">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-235">Updated: June 2017</span></span>

<span data-ttu-id="b0daa-236">[Den här guiden hittar du här][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="b0daa-236">[This guide can be found here][deployment-guide]</span></span>

### <span data-ttu-id="b0daa-237"><a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>DBMS Distributionsguide</span><span class="sxs-lookup"><span data-stu-id="b0daa-237"><a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>DBMS deployment guide</span></span>
<span data-ttu-id="b0daa-238">Rubrik: aaaAzure distribution av virtuella datorer DBMS för SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="b0daa-238">title: aaaAzure Virtual Machines DBMS deployment for SAP NetWeaver</span></span>

<span data-ttu-id="b0daa-239">Sammanfattning: Det här dokumentet beskrivs planering och implementering överväganden för hello DBMS system som ska köras tillsammans med SAP.</span><span class="sxs-lookup"><span data-stu-id="b0daa-239">Summary: This paper covers planning and implementation considerations for hello DBMS systems that should run in conjunction with SAP.</span></span> <span data-ttu-id="b0daa-240">Hello första delen, visas och presenteras allmänna överväganden.</span><span class="sxs-lookup"><span data-stu-id="b0daa-240">In hello first part, general considerations are listed and presented.</span></span> <span data-ttu-id="b0daa-241">hello handlar följande delar av hello dokumentet om toodeployments i olika DBMS i Azure som stöds av SAP.</span><span class="sxs-lookup"><span data-stu-id="b0daa-241">hello following parts of hello paper relate toodeployments of different DBMS in Azure that are supported by SAP.</span></span> <span data-ttu-id="b0daa-242">Olika DBMS som visas är SQL Server, SAP ASE och Oracle.</span><span class="sxs-lookup"><span data-stu-id="b0daa-242">Different DBMS presented are SQL Server, SAP ASE, and Oracle.</span></span> <span data-ttu-id="b0daa-243">I de specifika delarna beskrivs överväganden som du har tooaccount för när du kör SAP-system på Azure tillsammans med dessa DBMS.</span><span class="sxs-lookup"><span data-stu-id="b0daa-243">In those specific parts, considerations you have tooaccount for when you are running SAP systems on Azure in conjunction with those DBMS are discussed.</span></span> <span data-ttu-id="b0daa-244">Ämnen som metoder för säkerhetskopiering och hög tillgänglighet som stöds av hello olika DBMS på Azure presenteras för hello användning med SAP-program.</span><span class="sxs-lookup"><span data-stu-id="b0daa-244">Subjects like backup and high availability methods that are supported by hello different DBMS on Azure are presented for hello usage with SAP applications.</span></span>

<span data-ttu-id="b0daa-245">Uppdaterad: Juni 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-245">Updated: June 2017</span></span>

<span data-ttu-id="b0daa-246">[Den här guiden hittar du här][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="b0daa-246">[This guide can be found here][dbms-guide]</span></span>

### <a name="using-azure-site-recovery-for-sap-workload"></a><span data-ttu-id="b0daa-247">Med hjälp av Azure Site Recovery för SAP arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="b0daa-247">Using Azure Site Recovery for SAP workload</span></span>
<span data-ttu-id="b0daa-248">Rubrik: aaaSAP NetWeaver: skapa en lösning för katastrofåterställning med Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="b0daa-248">title: aaaSAP NetWeaver: Building a Disaster Recovery Solution with Azure Site Recovery</span></span> 

<span data-ttu-id="b0daa-249">Sammanfattning: Det här dokumentet beskriver hello sätt hur Azure Site Recovery services kan användas för hello syftet med hanteringen för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="b0daa-249">Summary: This document describes hello way how Azure Site Recovery services can be used for hello purpose of handling disaster recovery scenarios.</span></span> <span data-ttu-id="b0daa-250">Fall där Azure används som disaster recovery plats för en lokal SAP liggande med hjälp av Azure Site Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="b0daa-250">Cases where Azure is used as disaster recovery location for an on-premise SAP landscape using Azure Site Recovery Services.</span></span> <span data-ttu-id="b0daa-251">Ett annat scenario som beskrivs i hello dokumentet är hello Azure till Azure (A2A) disaster recovery fallet och hur den hanteras med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="b0daa-251">Another scenario described in hello document is hello Aure-to-Azure (A2A) disaster recovery case and how it is managed with Azure Site Recovery.</span></span>  

<span data-ttu-id="b0daa-252">Uppdaterad: Augusti 2017</span><span class="sxs-lookup"><span data-stu-id="b0daa-252">Updated: August 2017</span></span>

[<span data-ttu-id="b0daa-253">Den här guiden hittar du här</span><span class="sxs-lookup"><span data-stu-id="b0daa-253">This guide can be found here</span></span>](http://aka.ms/asr-sap)

