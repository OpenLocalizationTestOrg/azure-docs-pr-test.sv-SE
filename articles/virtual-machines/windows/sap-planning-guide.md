---
title: "SAP NetWeaver på virtuella Azure-datorer – planering och implementering | Microsoft Docs"
description: "SAP NetWeaver på virtuella Azure-datorer (VM) – planera Implementeringsguide och"
services: virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 2b58a419-c892-4722-8cb6-2f8beef4430c
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0b5968189512d3ca936c0e916274e1df057afb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="sap-netweaver-on-azure-windows-virtual-machines-vms--planning-and-implementation-guide"></a>SAP NetWeaver på Windows Azure virtuella datorer (VM) – planera Implementeringsguide och
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

[azure-cli]:../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:sap-dbms-guide.md
[dbms-guide-2.1]:sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
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

[deployment-guide]:sap-deployment-guide.md
[deployment-guide-2.2]:sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:sap-get-started.md
[getting-started-dbms]:sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:classic/sap-get-started.mdsap-dbms-guide.md
[getting-started-windows-classic-dbms]:classic/sap-get-started.mdsap-dbms-guide.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:classic/sap-get-started.mdsap-dbms-guide.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:classic/sap-get-started.mdsap-dbms-guide.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:classic/sap-get-started.mdsap-dbms-guide.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:classic/sap-get-started.mdsap-dbms-guide.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:sap-planning-guide.md
[planning-guide-1.2]:sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11.4]:sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

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
[planning-guide-microsoft-azure-networking]:sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[powershell-install-configure]:/powershell/azureps-cmdlets-docs
[resource-group-authoring-templates]:../../resource-group-authoring-templates.md
[resource-group-overview]:../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam
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
[virtual-machines-linux-cli-deploy-templates]:../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../linux/capture-image.md#step-2-create-vm-image
[virtual-machines-windows-capture-image]:../virtual-machines-windows-create-vm-generalized.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../virtual-machines-windows-create-vm-generalized.md
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

Microsoft Azure gör att företag kan få beräknings-och lagringsresurser minimal tidpunkt utan långa inköp cykler. Virtuella Azure-datorer tillåta företag att distribuera klassisk program som SAP NetWeaver baserat program i Azure och utöka sina tillförlitlighet och tillgänglighet utan att behöva ytterligare resurser tillgängliga lokalt. Azure Virtual Machine-tjänster har också stöd för korsanslutningar, som gör det möjligt att integrera aktivt Azure-datorer i sina lokala domäner sina privata moln och deras liggande för SAP-System.
I det här dokumentet beskriver grunderna i Microsoft Azure-dator och ger en beskrivning av överväganden för planering och implementering för SAP NetWeaver installationer i Azure och som sådana ska vara dokumentet att läsa innan du startar faktiska distributioner av SAP NetWeaver på Azure.
Dokumentet kompletterar SAP dokumentationen och SAP anteckningar som representerar primära resurser för installation och distribution av program på den angivna plattformar.

## <a name="summary"></a>Sammanfattning
Cloud Computing är en välanvänd term som ökat i vikt inom IT-branschen, från småföretag upp till stora multinationella bolag.

Microsoft Azure är Microsofts molntjänstplattform som erbjuder ett brett utbud av nya möjligheter. Nu kan kunder snabbt etablera och avinstallation etablera program som en tjänst i molnet, så att de inte är begränsat till teknisk eller budgetering begränsningar. Istället för att investera tid och budget i maskinvaruinfrastruktur, kan företag fokusera på programmet, verksamhetsprocesserna och dess fördelar för kunder och användare.

Med Microsoft Azure Virtual Machine-tjänsterna, erbjuder Microsoft en omfattande plattform för infrastruktur som en tjänst (IaaS). SAP NetWeaver-baserade program stöds på virtuella Azure-datorer (IaaS). Den här rapporten beskrivs hur du planerar och implementerar SAP NetWeaver baserat program i Microsoft Azure som plattform för serveroperativsystem.

Själva dokumentet fokuserar på två huvudsakliga aspekter:

* Den första delen beskriver två mönster för distribution som stöds för SAP NetWeaver baserat program i Azure. Allmän hantering av Azure med SAP-distribution i åtanke beskrivs också.
* Den andra delen redogör implementera två olika scenarier som beskrivs i den första delen.

För ytterligare resurser finns i kapitlet [resurser] [ planning-guide-1.2] i det här dokumentet.

### <a name="definitions-upfront"></a>Definitioner förskott
Vi använder följande termer för hela dokumentet:

* IaaS: Infrastruktur som en tjänst.
* PaaS: Plattform som en tjänst.
* SaaS: Programvara som en tjänst.
* ARM: Azure Resource Manager
* SAP komponent: ett enskilt SAP program, till exempel ECC BW, lösning Manager eller EP.  SAP-komponenter kan baseras på traditionell ABAP eller Java-teknik eller icke-NetWeaver baserat program, till exempel Business-objekt.
* SAP-miljö: en eller flera komponenter för SAP logiskt grupperade för att utföra en business-funktion, till exempel utveckling, QAS, träning, DR eller produktion.
* SAP liggande: Avser hela SAP tillgångarna i kundens IT-miljön. SAP-liggande innehåller alla produktions- och icke-produktionsmiljöer.
* SAP System: Kombinationen av DBMS lager- och programnivå t.ex. en SAP ERP-utvecklingssystemet, SAP BW testsystemet, SAP CRM produktionssystem osv... I Azure-distributioner går det inte att dela upp de här två lager mellan lokala och Azure. Detta innebär en SAP-systemet är antingen distribueras lokalt eller den har distribuerats i Azure. Du kan dock distribuera olika system för en SAP liggande i Azure eller lokalt. Du kan till exempel distribuera SAP CRM-utveckling och testsystem i Azure, men den SAP CRM produktion system lokalt.
* Endast molnbaserad distribution: en distribution där Azure-prenumerationen inte är ansluten via ett plats-till-plats eller en ExpressRoute-anslutning till lokala nätverkets infrastruktur. Gemensam dokumentation för Azure dessa typer av distributioner beskrivs också som 'endast molnbaserad-distributioner. Virtuella datorer distribueras med den här metoden kan nås via internet och en offentlig IP-adress och/eller en offentlig DNS-namn som tilldelats virtuella datorer i Azure. För Microsoft Windows lokala Active Directory (AD) och DNS inte har utökats till Azure i dessa typer av distributioner. De virtuella datorerna är därför inte en del av Active Directory lokalt. Detsamma gäller för Linux-implementeringar med t.ex. OpenLDAP + Kerberos.

> [!NOTE]
> Endast molnbaserad distributioner i det här dokumentet har definierats som slutförts SAP landskap kör enbart i Azure utan utökning av Active Directory / OpenLDAP eller namnmatchning från lokala i offentliga moln. Endast molnbaserad konfigurationer stöds inte för SAP produktionssystem eller konfigurationer där SAP STM eller andra lokala resurser måste användas mellan SAP-system som finns på Azure och resurser som finns lokalt.
>
>

* Anslutningar mellan lokala: Beskriver ett scenario där virtuella datorer distribueras till en Azure-prenumeration som har plats-till-plats, flera platser eller ExpressRoute-anslutningen mellan lokala datacenter(s) och Azure. Gemensamma Azure-dokumentationen dessa typer av distributioner beskrivs också som mellan lokala scenarier. Orsaken för anslutningen är att utöka lokala domäner lokala Active Directory / OpenLDAP och lokala DNS i Azure. Lokala liggande utökas till Azure tillgångar i prenumerationen. Med det här tillägget, kan de virtuella datorerna vara en del av den lokala domänen. Domänanvändare på den lokala domänen kan komma åt servrar och tjänster kan köra på dessa virtuella datorer (till exempel DBMS services). Det är möjligt kommunikation och namnmatchning mellan virtuella datorer som distribueras på lokalt och distribuerade virtuella Azure-datorer. Detta är det scenario som vi räknar med de flesta SAP-resurser som distribueras i.  Se [detta] [ vpn-gateway-cross-premises-options] artikel och [detta] [ vpn-gateway-site-to-site-create] för mer information.

> [!NOTE]
> Anslutningar mellan lokala distributioner av SAP-system där Azure virtuella datorer som kör SAP-system är medlemmar i en lokal domän stöds för produktion SAP-system. Anslutningar mellan lokala konfigurationer stöds för att distribuera delar eller slutföra SAP landskap till Azure. Även kör fullständig SAP liggande i Azure kräver att de virtuella datorer som en del av lokal domän och Active Directory / OpenLDAP. I tidigare versioner av dokumentationen talat om Hybrid-IT-scenarier där termen ”Hybrid' är rotad i det faktum att det finns en anslutning mellan Azure och lokalt. Dessutom att virtuella datorer i Azure är en del av Active Directory lokalt / OpenLDAP.
>
>

Vissa Microsoft-dokumentationen beskriver anslutningar mellan lokala scenarier lite annorlunda särskilt för DBMS HA konfigurationer. När det gäller SAP dokument i scenarion med flera lokaler bara handlar i att ha en anslutning för plats-till-plats eller privat (ExpressRoute) och det faktum att SAP-liggande fördelas mellan lokala och Azure.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Resurser
Följande ytterligare guider är tillgängliga för avsnittet SAP-distribution på Azure:

* [SAP NetWeaver på Azure Virtual Machines (virtuella datorer) – planering och Implementation Guide (det här dokumentet)][planning-guide]
* [SAP NetWeaver på virtuella Azure-datorer (VM) – Distributionsguide][deployment-guide]
* [SAP NetWeaver på virtuella Azure-datorer (VM) – DBMS Distributionsguide][dbms-guide]
* [SAP NetWeaver på virtuella Azure-datorer (VM) – Distributionsguide för hög tillgänglighet][ha-guide]

> [!IMPORTANT]
> Om det är möjligt en länk till den refererande SAP-installationsguiden används (referens InstGuide-01, se <http://service.sap.com/instguides>). När det gäller kraven och processen för installation, bör SAP NetWeaver installationen guider alltid läsas noggrant, som det här dokumentet beskriver endast specifika uppgifter för SAP NetWeaver system som är installerade i en Microsoft Azure-dator.
>
>

Följande information för SAP är relaterade till avsnittet SAP i Azure:

| Nummer | Rubrik |
| --- | --- |
| [1928533] |SAP-program i Azure: produkter och storlek som stöds |
| [2015553] |SAP på Microsoft Azure: stöd för krav |
| [1999351] |Felsökning av utökad Azure övervakning för SAP |
| [2178632] |Key som övervakning mått för SAP på Microsoft Azure |
| [1409604] |Virtualisering på Windows: förbättrad övervakning |
| [2191498] |SAP på Linux med Azure: förbättrad övervakning |
| [2243692] |Linux på Microsoft Azure (IaaS) VM: SAP problem |
| [1984787] |SUSE LINUX Enterprise Server 12: Installationsinformation |
| [2002167] |Red Hat Enterprise Linux 7.x: Installation och uppgradering |

Läs även den [Tillståndsändringsavisering Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) som innehåller alla SAP anteckningar för Linux.

Allmän standardbegränsningar och maximala begränsningar i Azure-prenumerationer finns i [i den här artikeln][azure-subscription-service-limits-subscription]

## <a name="possible-scenarios"></a>Möjliga scenarier
SAP visas ofta som en av de mest kritiska program i företag. Arkitektur och drift av dessa program är främst mycket komplex och det är viktigt att se till att du uppfyller kraven på tillgänglighet och prestanda.

Företag har därför Tänk noggrant om vilka program kan köras i ett offentligt moln miljö, oberoende av valda molnleverantören.

Möjliga typer för att distribuera SAP NetWeaver baserat program i offentliga moln miljöer visas nedan:

1. Medelhög storlek produktionssystem
2. Utvecklingssystem
3. Testa system
4. Prototyp system
5. Learning / Demonstration system

För att distribuera SAP-system i Azure IaaS eller IaaS i allmänhet, är det viktigt att förstå de viktiga skillnaderna mellan erbjudanden för traditionella outsourcers eller värdar och IaaS erbjudanden. Traditionella värden eller outsourcer anpassas automatiskt infrastruktur (nätverk, lagring och server-typ) till en kund vill vara värd för arbetsbelastningen, är det i stället kundens ansvar att välja rätt arbetsbelastningen för IaaS-distributioner.

Som ett första steg bör måste kunder du kontrollera följande:

* SAP stöds VM typer av Azure
* SAP produkter/versioner som stöds på Azure
* Stöds operativsystem och DBMS utgåvor specifika SAP-versioner i Azure
* SAP-genomströmning som tillhandahålls av olika Azure-SKU: er

Svaren på dessa frågor kan läsas i SAP-kommentar [1928533].

Som ett andra steg måste Azure begränsningar för resursen och bandbredd som ska jämföras med faktiska resurser som används för lokala system. Kunder måste känna till de olika funktionerna i Azure typer som stöds med SAP i området:

* Processor och minne resurser av VM av olika typer och
* IOPS bandbredd på VM av olika typer och
* Nätverksfunktioner för VM av olika typer.

De flesta av dessa data kan hittas [här][virtual-machines-sizes]

Tänk på att gränserna som anges i länken ovan är den övre gränsen. Det innebär inte att gränserna för någon av resurser, t.ex. IOPS kan anges i alla fall. Undantagen är dock CPU och minne resurser av typen valda VM. För VM-typer som stöds av SAP, är CPU och minne resurserna reserverade och som sådan är tillgängliga när som helst i tid för användning i den virtuella datorn.

Microsoft Azure-plattformen som andra plattformar, IaaS är flera innehavare. Det innebär att lagring, nätverk och andra resurser delas mellan klienter. Intelligent begränsning och kvot logik används för att förhindra att en klient påverka prestanda hos en annan klient (störningar granne) på drastiskt sätt. Även om logiken i Azure försöker används keep avvikelser i bandbredd orsakade ett litet, hög delade plattformar tenderar att införa större avvikelser i resursen/bandbreddstillgänglighet än många kunder att i sina lokala distributioner. Därför kan det uppstå olika nivåer av bandbredd angående nätverks- eller i/o (volym samt svarstid) från en minut till en minut. Sannolikheten att ett SAP-system i Azure kan uppleva större avvikelser än i ett lokalt system måste beaktas.

Sista steget är att utvärdera tillgänglighet. Det kan inträffa att underliggande Azure-infrastrukturen behöver uppdateras och kräver värdar som kör virtuella datorer startas om. I dessa fall skulle virtuella datorer som körs på värdarna stängs av och startas också. Tidtagningen av sådana Underhåll görs under-core kontorstid för en viss region men fönstret potentiella några timmar under vilka en omstart kommer att ske är relativt stort. Det finns olika tekniker i Azure-plattformen som kan konfigureras för att åtgärda vissa eller alla effekten av dessa uppdateringar. Framtida förbättringar av Azure-plattformen DBMS och SAP-program är utformade för att minimera effekten av sådana omstarter.

För att distribuera en SAP-systemet till Azure, lokal SAP system operativsystem, databas och SAP-program måste visas på supportmatrisen SAP Azure passar med resurserna som kan ge Azure-infrastrukturen och som kan arbeta med Tillgänglighet SLA Microsoft Azure erbjuder. Eftersom dessa system identifieras måste du bestämma på något av följande två distributionsscenarier.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Endast molnbaserad - distributioner på virtuella datorer till Azure utan beroenden på lokala kundens nätverk
![Enskild virtuell dator med SAP demo eller utbildning scenario distribueras i Azure][planning-guide-figure-100]

Det här scenariot är typiska för kurser eller demo system där alla komponenter för SAP och icke-program installeras på en enskild virtuell dator. SAP produktionssystem stöds inte i det här distributionsscenariot. I allmänhet är uppfyller det här scenariot följande krav:

* Virtuella datorerna är tillgängliga via ett offentligt nätverk. Direkta nätverksanslutningar för program som körs i de virtuella datorerna på det lokala nätverket för antingen företaget äger demonstrationer eller utbildningar innehåll eller kunden är inte nödvändigt.
* Vid flera virtuella datorer som representerar utbildningar eller demo scenario, måste nätverket kommunikation och namnmatchning fungerar mellan de virtuella datorerna. Men kommunikation mellan en uppsättning virtuella datorer måste isoleras så att flera uppsättningar med virtuella datorer kan distribueras sida vid sida utan störningar.  
* Internetanslutning krävs för att slutanvändaren kan fjärrinloggning till att de virtuella datorerna finns i Azure. Beroende på gästen används OS, Terminal Services/RDS eller VNC/ssh för att komma åt den virtuella datorn att utföra uppgifter för utbildning eller utföra demonstrationer. Om SAP-portarna som 3200, 3300 och 3600 kan också exponeras programinstansen SAP kan nås från alla anslutna Internet-skrivbordet.
* SAP system (och VM(s)) representerar ett fristående scenario i Azure som endast kräver en offentlig Internetanslutning för slutanvändaren åtkomst och inte kräver en anslutning till andra virtuella datorer i Azure.
* SAPGUI och en webbläsare installeras och köras direkt på den virtuella datorn.
* Det krävs en snabb återställning av en virtuell dator till det ursprungliga tillståndet och ny distribution av det ursprungliga tillståndet igen.
* När det gäller demonstration och utbildning scenarier som realiseras i flera virtuella datorer, en Active Directory / OpenLDAP och/eller DNS-tjänsten krävs för varje uppsättning virtuella datorer.

![Grupp med Virtuella datorer som representerar en demo eller utbildning scenariot i en Azure-molntjänst][planning-guide-figure-200]

Det är viktigt att tänka på att virtuella datorer i varje typ måste distribueras parallellt, där namnen VM i varje uppsättningen är samma.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>anslutningar mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med krav på att fullständigt integrerat i det lokala nätverket
![VPN-anslutningar med plats-till-plats-anslutning (mellan platser)][planning-guide-figure-300]

Det här scenariot är en scenarion med flera lokaler med många möjlig distribution mönster. Det kan beskrivas så enkelt sätt som kör vissa delar av SAP liggande lokalt och andra delar av SAP liggande på Azure. Alla aspekter av det faktum som en del av SAP-komponenter körs på Azure ska vara transparent för slutanvändare. Därför fungerar SAP Transport Correction System (STM), RFC kommunikation, utskrift, säkerhet (till exempel SSO), etc. sömlöst för SAP-system som körs på Azure. Men i scenarion med flera lokaler beskriver också ett scenario där fullständig SAP liggande körs i Azure med kundens domänen och DNS utökats till Azure.

> [!NOTE]
> Detta är det distributionsscenariot som stöds för att köra produktiva SAP-system.
>
>

Läs [i den här artikeln] [ vpn-gateway-create-site-to-site-rm-powershell] mer information om hur du ansluter din lokala nätverk till Microsoft Azure

> [!IMPORTANT]
> När vi pratar om anslutningar mellan lokala scenarier mellan Azure och lokala kunddistributioner tittar vi Granulariteten för hela SAP-system. Scenarier som är *stöds inte* för anslutningar mellan lokala scenarier är:
>
> * Olika lager i SAP-program som körs i olika distributionsmetoder. T.ex. köra den DBMS layer lokalt, men programnivå SAP i virtuella datorer som distribueras som virtuella Azure-datorer eller tvärtom.
> * Vissa komponenter i ett SAP-lager i Azure och vissa lokalt. T.ex. dela upp instanser av SAP Applikationsnivån mellan lokala och virtuella Azure-datorer.
> * Distribution av virtuella datorer som kör SAP instanser av ett system över flera Azure-regioner stöds inte.
>
> Orsaken till dessa begränsningar är krav för ett nätverk med mycket låg svarstid höga prestanda i en SAP-systemet, särskilt mellan programinstanser och DBMS-lager av ett SAP-system.
>
>

### <a name="supported-os-and-database-releases"></a>OS- och databasen versioner som stöds
* Microsoft server-program stöds för virtuella Azure-tjänster ingår i den här artikeln: <http://support.microsoft.com/kb/2721672>.
* Operativsystem system-versioner som stöds, databasen system-versioner som stöds på virtuella Azure-tjänster tillsammans med SAP-program finns dokumenterade i SAP-kommentar [1928533].
* SAP-program och versioner som stöds på virtuella Azure-tjänster som finns dokumenterade i SAP-kommentar [1928533].
* Endast 64-bitarsavbildningar stöds för att köras som gäst-VM i Azure för SAP-scenarier. Det innebär att endast 64-bitars SAP-program och databaser stöds.

## <a name="microsoft-azure-virtual-machine-services"></a>Virtuella Microsoft Azure-tjänster
Microsoft Azure-plattformen är en internet-skala molnplattform finns och drivas på ett Microsoft-datacenter. Plattformen som inkluderar Microsoft Azure Virtual Machine-tjänster (infrastruktur som en tjänst eller IaaS) och en uppsättning omfattande plattform som en tjänst (PaaS)-funktioner.

Azure-plattformen minskar behovet av direkta teknik och infrastruktur inköp. Det förenklar underhåll och drift av program genom att tillhandahålla på begäran beräkning och lagring till värdar, skala och hantera webbprogram och anslutna program. Infrastrukturhantering är automatisk med en plattform som är avsedd för hög tillgänglighet och dynamisk skalning för att matcha användarbehov med alternativet för en modell med betalning per användning prisnivå.

![Placering av virtuella Microsoft Azure-tjänster][planning-guide-figure-400]

Med Azure-tjänster för virtuella datorer, Microsoft för att du kan distribuera anpassade server-avbildningar till Azure som IaaS instanser (se figur 4). De virtuella datorerna i Azure baseras på Hyper-V virtuella hårddiskar (VHD) och kunna kör olika operativsystem som Gästoperativsystem.

Ur ett operativa erbjuder virtuella Azure-tjänsten liknande åtgärder som virtuella datorer som distribuerats lokalt. Men har betydande fördelar som du inte behöver köpa, administrera och hantera infrastrukturen. Utvecklare och administratörer har fullständig kontroll av operativsystemavbildningen i dessa virtuella datorer. Administratörer kan logga in via en fjärranslutning till de virtuella datorerna för att utföra underhåll och felsökning aktiviteter samt programvara distributionsåtgärder. Om distributionen är endast begränsningarna storlekar och funktionerna i virtuella Azure-datorer. Dessa kanske inte lika bra detaljerade i konfigurationen som detta kan göras lokalt. Det finns ett val av VM-typer som representerar en kombination av:

* Antal vCPUs,
* Minne,
* Antal virtuella hårddiskar som kan kopplas
* Nätverk och lagring bandbredder.

Storlek och begränsningar med olika storlekar för olika virtuella datorer som kan ses i en tabell i [i den här artikeln][virtual-machines-sizes]

När du uppnår finns det olika familjer eller antal virtuella datorer. Från och med Dec 2015, kan du skilja familjerna för virtuella datorer:

* A0 A7 VM typer: inte alla de som är certifierade för SAP. Första VM-serien som Azure IaaS fick introducerades i.
* A8 A11 VM typer: höga prestanda instanser. Körs på olika bättre utför beräkna värddatorer än andra A-series virtuella datorer.
* D-serien VM typer: utför bättre än A0 A7. Inte alla VM-typer är certifierade med SAP.
* DS-serien VM typer: använda samma värdar som D-serien, men kan ansluta till Azure Premium-lagring (finns i kapitlet [Azure Premium Storage] [ planning-guide-3.3.2] i det här dokumentet). Igen inte alla VM-typer som är certifierade med SAP.
* G-serien VM typer: höga minnesområdet VM-typer.
* GS-serien VM typer: som G-serien men med alternativet att använda Azure Premium-lagring (finns i kapitlet [Azure Premium Storage] [ planning-guide-3.3.2] i det här dokumentet). När du använder GS-serien virtuella datorer som databasservrar är det obligatoriskt att använda Premium-lagring för DB data och transaktionen loggfiler

Du hittar samma Processorn och RAM-minne i olika virtuella dator-serien. Dock när du letar efter kapaciteten på dessa virtuella datorer utanför annan serie de skiljer sig avsevärt. Trots att ha samma konfiguration för CPU och minne. Orsaken är att den underliggande värd servermaskinvaran vid införandet av de olika typerna av VM hade olika dataflödesegenskaper.  Vanligtvis återspeglas skillnaden visas i kapaciteten också i priset för olika virtuella datorer.

Observera att inte alla olika VM-serien kan erbjuds i var och en av de Azure-regioner (för Azure-regioner finns nästa kapitel). Tänk också att inte alla virtuella datorer eller VM-serien är certifierad för SAP.

> [!IMPORTANT]
> Användning av SAP NetWeaver baserat program, endast del av VM-typer och konfigurationer som anges i SAP-kommentar [1928533] stöds.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-regioner
Microsoft kan distribuera virtuella datorer i så kallade ”Azure-regioner”. En Azure-Region kanske en eller flera datacenter som befinner sig i närheten. Microsoft har minst två Azure-regioner för de flesta av de geopolitiska regionerna i världen. T.ex. Det finns en Azure-Region 'Nordeuropa, och ett av 'Västeuropa' i Europa. Dessa två Azure-regioner inom en geopolitiska region avgränsas med tillräckligt betydande avstånd så att naturliga eller tekniska katastrofer inte påverkar både Azure-regioner i samma geopolitiska region. Eftersom Microsoft bygger stadigt ut nya Azure-regioner i olika geopolitiska regioner globalt, växande ständigt antal dessa regioner och från och med Dec 2015 uppnått antalet 20 Azure-regioner med ytterligare områden som redan har presenterat. I alla dessa regioner, inklusive de två regionerna Azure i Kina kan du som kund distribuera SAP-system. För aktuella uppdaterade information om Azure-regioner finns i den här webbplatsen: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Begreppet virtuella Microsoft Azure
Microsoft Azure tillhandahåller en infrastruktur som en tjänst (IaaS) till värd för virtuella datorer med liknande funktioner som ett lokalt virtualiseringslösning. Du kan skapa virtuella datorer från i Azure Portal, PowerShell eller CLI, som också ger distribution och hanteringsfunktioner.

Med Azure Resource Manager kan du etablera dina program med hjälp av en deklarativ mall. I samma mall kan du distribuera flera tjänster tillsammans med deras beroenden. Du kan använda samma mall för att upprepade gånger distribuera programmet under varje steg i livscykeln för programmet.

Mer information om hur du använder ARM-mallar finns här:

* [Distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och Azure CLI][virtual-machines-linux-cli-deploy-templates]
* [Hantera virtuella datorer med Azure Resource Manager och PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/Documentation/Templates/>

En annan intressant funktion är möjligheten att skapa avbildningar av virtuella datorer, där du kan förbereda vissa databaser som du kan snabbt distribuera instanser för virtuella datorer som uppfyller dina krav.

Mer information om hur du skapar bilder från virtuella datorer finns i [i den här artikeln (Windows)] [ virtual-machines-windows-capture-image] eller i [(Linux) för den här artikeln] [ virtual-machines-linux-capture-image].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Feldomäner
Feldomäner representerar en fysisk enhet skulle misslyckas, mycket nära relaterade till den fysiska infrastrukturen i datacenter och när en fysisk bladet eller rack kan betraktas som en Feldomän finns det ingen direkt mappningen mellan två.

Du kan påverka Azure-Infrastrukturkontrollanten att distribuera programmet till olika Feldomäner, vilket uppfyller kraven i Microsoft Azure-serviceavtalet när du distribuerar flera virtuella datorer som en del av en SAP-system i virtuella Microsoft Azure-tjänster. Distribution av Feldomäner i en Azure-Skalningsenhet (insamling av hundratals datornoder eller lagringsnoder och nätverk) eller tilldelning av virtuella datorer till en specifik Feldomän är dock något som du inte har direkt kontroll. För att kunna dirigera Azure-infrastrukturkontrollanten att distribuera en uppsättning virtuella datorer via olika Feldomäner, måste du tilldela en Azure Tillgänglighetsuppsättning till de virtuella datorerna vid tidpunkten för distribution. Mer information om Azure-Tillgänglighetsuppsättningar finns i kapitlet [Azure-Tillgänglighetsuppsättningar] [ planning-guide-3.2.3] i det här dokumentet.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Uppgradera domäner
Uppgraderingsdomäner representerar en logisk enhet som hjälp för att avgöra hur en virtuell dator i en SAP-system som består av SAP-instanser som körs i flera virtuella datorer kommer att uppdateras. När en uppgradering sker går Microsoft Azure igenom processen för att uppdatera dessa Uppgraderingsdomäner i taget. Du kan skydda SAP systemet delvis från potentiell nedtid genom att sprida VMs vid tidpunkten för distribution över olika uppgradera domäner. Du måste ange ett specifikt attribut vid tidpunkten för distribution av varje virtuell dator för att tvinga Azure för att distribuera de virtuella datorerna i ett SAP-system som är utspridda över olika uppgradera domäner. Liknar Feldomäner, en Skalningsenhet för Azure är indelade i flera uppgradera domäner. För att kunna dirigera Azure-infrastrukturkontrollanten att distribuera en uppsättning virtuella datorer via olika uppgradera domäner, måste du tilldela en Azure Tillgänglighetsuppsättning till de virtuella datorerna vid tidpunkten för distribution. Mer information om Azure-Tillgänglighetsuppsättningar finns i kapitlet [Azure-Tillgänglighetsuppsättningar] [ planning-guide-3.2.3] nedan.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure Tillgänglighetsuppsättningar
Azure virtuella datorer i en Tillgänglighetsuppsättning av typen Azure kommer att distribueras av Azure-Infrastrukturkontrollanten över olika fel- och uppgradera domäner. Syftet med distributionen över olika fel- och uppgradera domäner är att förhindra att alla virtuella datorer i en SAP-systemet avslutas när det gäller infrastrukturens underhåll eller ett fel i en Feldomän. Som standard ingår virtuella datorer inte i en Tillgänglighetsuppsättning. Deltagande i en virtuell dator i en Tillgänglighetsuppsättning har definierats vid tidpunkten för distribution eller vid ett senare tillfälle genom en omkonfiguration och en ny distribution av en virtuell dator.

För att förstå begreppet Azure-Tillgänglighetsuppsättningar och hur Tillgänglighetsuppsättningar relaterar till fel och uppgradera domäner, Läs [i den här artikeln][virtual-machines-manage-availability]

Om du vill definiera tillgänglighetsuppsättningar för ARM via en json-mall finns [rest-api-specifikationerna](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) och Sök efter ”tillgänglighet”.

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Lagring: Microsoft Azure Storage- och Datadiskar
Microsoft Azure virtuella datorer använda olika lagringstyper. När du implementerar SAP på Azure-tjänster för virtuella datorer är det viktigt att förstå skillnaderna mellan dessa två typer av lagring:

* Icke-beständig, temporär lagring.
* Beständig lagring.

Icke-beständig lagring är direkt anslutna till de virtuella datorerna och finns på datornoderna själva – lokal instans lagring (tillfällig lagring). Storleken beror på storleken på den virtuella datorn som valdes när distributionen är igång. Den här lagringstypen är temporär och därför disken initieras när en virtuell dator-instansen startas om. Växlingsfil för operativsystemet finns normalt på tillfälliga disken.

- - -
> ![Windows][Logo_Windows] Windows
>
> Temporär enheten är monterad enhet D:\ i en distribuerad virtuell dator på virtuella Windows-datorer.
>
> ![Linux][Logo_Linux] Linux
>
> Den är monterad som /mnt/resource eller /mnt på virtuella Linux-datorer. Se mer information här:
>
> * [Hur du kopplar en datadisk till en virtuell Linux-dator][virtual-machines-linux-how-to-attach-disk]
> * <http://blogs.msdn.com/b/mast/Archive/2013/12/07/Understanding-the-Temporary-Drive-on-Windows-Azure-Virtual-Machines.aspx>
>
>

- - -
Den faktiska enheten är temporära eftersom den hämtar lagras på värdservern sig själv. Om den virtuella datorn flyttas i en omdistribution (t.ex. på grund av underhåll på värden eller avstängning och omstart) går innehållet i enheten förlorad. Det är därför inte ett alternativ för att lagra viktiga data på den här enheten. Typ av media som används för den här typen av lagring skiljer sig mellan olika virtuella dator-serien med mycket olika prestandaegenskaper som från och med juni 2015 ser ut som:

* A5 A7: Mycket begränsad prestanda. Rekommenderas inte för något utöver växlingsfilen
* A8-A11: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.
* D-serien: Goda prestandaegenskaper med vissa sedan tusen IOPS och > 1 GBIT/s genomströmning.
* DS-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.
* G-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.
* GS-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.

Uttryck ovanför tillämpas för VM-typer som är certifierade med SAP. VM-serien med utmärkt IOPS och genomströmning uppfyller kraven för att dra nytta av vissa DBMS-funktioner. Finns det [DBMS Deployment Guide] [ dbms-guide] för mer information.

Microsoft Azure Storage innehåller beständiga lagring och vanliga nivåer av skydd och redundans på SAN-lagring. Diskar baserat på Azure Storage är en virtuell hårddisk (VHD) finns i Azure Storage-tjänster. Den lokala OS-disken (Windows C:\, Linux / (/ dev/sda1)) lagras på Azure-lagring och ytterligare volymer/diskar som monterats på den virtuella datorn lagras där, för.

Det är möjligt att överföra en befintlig virtuell Hårddisk från lokala eller skapar en tom från i Azure och koppla dem till distribuerade virtuella datorer. Refererar till de virtuella hårddiskarna som Azure-diskar.

När du skapar eller ladda upp en VHD till Azure Storage, är det möjligt att montera och koppla dem till en befintlig virtuell dator och kopiera befintliga (omonterade) VHD.

Data och ändringar i de är säkra när startas om och återskapa en virtuell datorinstans som de virtuella hårddiskarna är beständiga. Även om en instans har tagits bort kan dessa virtuella hårddiskar säkerhet och kan distribueras eller vid ej OS diskar kan monteras till andra virtuella datorer.

Nivåer för olika redundans kan konfigureras i nätverket för Azure Storage:

* Lägsta nivå som kan väljas är 'lokala redundans', vilket motsvarar tre replik av data i samma Datacenter för en Azure-Region (finns i kapitlet [Azure-regioner][planning-guide-3.1]).
* Zonen redundant lagring som ska spridas tre bilder över olika resurser inom samma Azure-Region.
* Standardnivån för redundans är geografisk redundans som replikerar asynkront innehållet till en annan 3 bilder av data till en annan Azure-Region som finns i samma geopolitiska region.

Också se tabellen på den här artikeln angående alternativen olika redundans: <https://azure.microsoft.com/pricing/details/storage/>

Mer information angående Azure Storage finns här:

* <https://Azure.microsoft.com/Documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/site-Recovery>
* <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-disk-Encryption-for-Linux-and-Windows-Virtual-Machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Azure standardlagring
Azure blobblagring som Standard var typ av lagring som är tillgängliga när Azure IaaS släpptes. Det fanns IOPS kvoter tillämpas per enskild VHD. Latens som uppstod var inte i samma klass som SAN/NAS-enheter som normalt distribueras för avancerade SAP-datorer lagras lokalt. Dock Azure standardlagring visat sig vara tillräcklig för flera hundra SAP-system under tiden distribueras i Azure.

Azure standardlagring debiteras baserat på de faktiska data som lagras, mängden lagringstransaktioner och utgående dataöverföringar redundans-alternativ som valts. Många virtuella hårddiskar kan skapas på den högsta 1TB i storlek, men så länge de förblir tom är kostnadsfri. Om du fyller sedan en virtuell Hårddisk med 100GB, debiteras du för att lagra 100GB och inte för den virtuella Hårddisken har skapats med nominell storlek.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium-lagring
I April 2015 introducerade Microsoft Azure Premium-lagring. Premium-lagring har fått introducerades med målet att tillhandahålla:

* Bättre i/o-svarstid.
* Bättre genomflöde.
* Mindre variationer i i/o-svarstid.

För detta ändamål introducerades många ändringar av två viktigaste är:

* Användning av SSD-diskar i Azure Storage-noder
* En ny läsa cache som backas upp av en Azure compute-nod lokal SSD

I motsatt standardlagring där funktioner inte ändras beroende på storleken på disken (eller VHD), Premium-lagring har för närvarande 3 annan disk kategorier som visas i slutet av den här artikeln innan avsnittet vanliga frågor och svar: <https:// Azure.microsoft.com/pricing/details/Storage/>

Du ser att IOPS/VHD- och diskresurser genomströmning/VHD är beroende av kategorin storlek på diskar

Kostnad grunden för Premium-lagring är inte faktiska datavolym som lagras i dessa virtuella hårddiskar, men kategorin storleken på sådana en virtuell Hårddisk, oberoende av mängden data som lagras i den virtuella Hårddisken.

Du kan också skapa virtuella hårddiskar på Premium-lagring som inte direkt mappning till storlek kategorier som visas. Detta kan vara fallet, särskilt när du kopierar virtuella hårddiskar från standardlagring till Premium-lagring. I sådana fall utförs en mappning till nästa största Premium-lagring disk alternativ.

Tänk på att bara vissa VM-serien kan dra nytta av Azure Premium-lagring. Från och med Dec 2015 är dessa DS - och GS-serien. DS-serien är i princip samma D-serien med undantaget att DS-serien har möjlighet att montera Premium-lagring baseras VMs dessutom till virtuella hårddiskar som finns på Azure standardlagring. Samma sak gäller för G-serien och GS-serien.

Om du checkar ut del av DS-serien virtuella datorer i [i den här artikeln] [ virtual-machines-sizes] du ska vara medveten om att det finns data volym begränsningar Premium Storage virtuella hårddiskar på Granulariteten för VM-nivå. Olika DS-serien eller GS-serien virtuella datorer har också olika begränsningar för antalet virtuella hårddiskar som kan monteras angående. Dessa gränser dokumenteras i artikeln som nämns ovan samt. Men i princip betyder att om du t.ex. montera 32 x P30 diskar/VHD: er till en enda DS14 VM du inte får 32 x maximalt dataflöde för en P30 disk. I stället begränsar maximalt dataflöde på VM-nivå enligt beskrivningen i artikeln överföring av data.

Mer information om Premium-lagring finns här: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure Storage-konton
När du distribuerar tjänster eller virtuella datorer i Azure, ordnas distribution av virtuella hårddiskar och VM-avbildningar i enheter som kallas Azure Storage-konton. När du planerar en distribution av Azure måste du noga överväga begränsningar för Azure. Det finns ett begränsat antal Lagringskonton per Azure-prenumeration på en sida. Även om varje Azure Storage-konto kan innehålla ett stort antal VHD-filer, finns det en fast gräns för totala IOPS per Lagringskonto. När du distribuerar hundratals SAP virtuella datorer med DBMS system skapar betydande i/o-anrop, rekommenderas att distribuera hög IOPS DBMS VMs mellan flera Azure Storage-konton. Vara måste försiktig så att inte överskrida den aktuella gränsen för Azure Storage-konton per prenumeration. Eftersom lagringen är en viktig del av distributionen databasen för en SAP-system, detta begrepp beskrivs i detalj i redan refereras [DBMS Deployment Guide][dbms-guide].

Mer information om Azure Storage-konton finns i [i den här artikeln][storage-scalability-targets]. Läsa den här artikeln kommer inser du att det finns skillnader i begränsningar mellan Lagringskonton i Azure-Standard och Premium-Lagringskonton. Viktiga skillnader är mängden data som kan lagras i ett Lagringskonto. Volymen är en omfattning som är större än med Premium-lagring i standardlagring. På andra sidan är standardkontot för lagring mycket begränsad IOPS (se kolumnen 'Totala begära hastigheten'), medan Azure Premium Storage-konto har ingen sådan begränsning. Diskuteras information och resultatet av dessa skillnader när du ska diskutera distributioner av SAP-system, särskilt DBMS-servrar.

Du har möjlighet att skapa olika behållare för att ordna och kategorisera olika virtuella hårddiskar i ett Lagringskonto. Dessa behållare används vanligtvis för att till exempel separata virtuella hårddiskar på olika virtuella datorer. Det finns inga konsekvenser för prestanda i med hjälp av en behållare eller flera behållare under en enda Azure Storage-konto.

I Azure följer en VHD-namnet följande namngivning anslutning som måste ange ett unikt namn för den virtuella Hårddisken i Azure:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Som nämnts ovan strängen måste identifiera den virtuella Hårddisken som lagras på Azure Storage.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure-nätverk
Microsoft Azure tillhandahåller en nätverksinfrastruktur, vilket gör att mappningen av alla scenarier som vi vill utnyttja med SAP-program. Funktionerna är:

* Åtkomst från utanför, direkt till de virtuella datorerna via Windows Terminal Services eller ssh/VNC
* Åtkomst till tjänster och specifika portar som används av program inom de virtuella datorerna
* Intern kommunikation och namnmatchning mellan en grupp med virtuella datorer som distribueras som virtuella Azure-datorer
* Anslutning mellan en kund lokala nätverk och Azure-nätverk
* Mellan Azure-Region eller data center anslutning mellan Azure platser

Mer information hittar du här: <https://azure.microsoft.com/documentation/services/virtual-network/>

Det finns en mängd olika alternativ för att konfigurera namn och IP-lösning i Azure. I detta dokument, endast molnbaserad scenarier som förlitar sig på standardvärdet med hjälp av Azure DNS (i stället för att definiera en egen DNS-tjänst). Det finns också en ny Azure DNS-tjänst som kan användas i stället för att ställa in DNS-servern. Mer information finns i [i den här artikeln] [ virtual-networks-manage-dns-in-vnet] på [den här sidan](https://azure.microsoft.com/services/dns/).

För anslutningar mellan lokala scenarier vi beroende av faktumet som lokalt DNS-AD/OpenLDAP har utökats via VPN eller privat anslutning till Azure. För vissa scenarier som dokumenteras här det kan vara nödvändigt att ha en replik av en AD/OpenLDAP installerad i Azure.

Eftersom nätverk och namnmatchning är en viktig del av distributionen databasen för en SAP-system, beskrivs i detalj i detta begrepp i [DBMS Deployment Guide][dbms-guide].

##### <a name="azure-virtual-networks"></a>Azure Virtual Networks
Du kan definiera adressintervallet privata IP-adresser som allokerats av Azure DHCP-funktioner genom att bygga upp ett Azure Virtual Network. I scenarier med anslutningar mellan lokala allokeras IP-adressintervall som definierats fortfarande med hjälp av DHCP av Azure. Domän-namnmatchning utförs lokalt (förutsatt att de virtuella datorerna är en del av en lokal domän) och därför kan matcha adresser utöver olika Azure-molntjänster.

[comment]: <> (MSSedusch fortfarande behövs? TODO ursprungligen ett Azure Virtual Network är bunden till en Tillhörighetsgrupp. Ett virtuellt nätverk i Azure med som har begränsad till Azure-Skalningsenhet som Tillhörighetsgruppen har tilldelats. Detta innebar i slutändan det virtuella nätverket har begränsad till de tillgängliga resurserna i Azure Skalningsenheten. Detta har ändrats sedan och nu virtuella Azure-nätverk kan sträcka ut över flera Azure-Skalningsenhet. Dock som kräver att virtuella Azure-nätverk är ** inte ** som är associerade med Tillhörighetsgrupper längre vid skapandet. Vi som nämnts tidigare att i motsatt rekommendationer per år sedan, bör du ** inte längre använda Azure-Tillhörighetsgrupper **. Mer information finns < https://azure.microsoft.com/blog/regional-virtual-networks/>)

Varje virtuell dator i Azure måste du vara ansluten till ett virtuellt nätverk.

Mer information finns i [i den här artikeln] [ resource-groups-networking] på [den här sidan](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd TODO gick inte att hitta en artikel som innehåller OpenLDAP avsnittet + ARM;)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Som standard när en virtuell dator distribueras ändra du inte konfigurationen för virtuella nätverk. TCP/IP-inställningar måste finnas kvar i Azure DHCP-server. Standardbeteendet är dynamisk IP-tilldelning.
>
>

MAC-adressen för det virtuella nätverkskortet kan t.ex. ändras efter att ändra storlek och gästen Windows eller Linux OS ska hämta nytt nätverkskort och använder automatiskt DHCP för att tilldela IP-adress och DNS-adresser i det här fallet.

##### <a name="static-ip-assignment"></a>Statiska IP-tilldelning
Det är möjligt att tilldela fast eller reserverade IP-adresser till virtuella datorer i ett Azure Virtual Network. Kör de virtuella datorerna i ett Azure Virtual Network öppnas en bra möjlighet att utnyttja den här funktionen om behövs eller krävs för vissa scenarier. IP-adresstilldelning fortfarande är giltiga i hela förekomsten av den virtuella datorn, oberoende av om den virtuella datorn är igång eller avstängning. Därför måste du göra det totala antalet virtuella datorer (VM körs och stoppas) i beräkningen när du definierar IP-adressintervall för det virtuella nätverket. IP-adressen förblir tilldelade antingen tills den virtuella datorn och dess nätverksgränssnittet har tagits bort eller IP-adressen hämtar Frigör tilldelade igen. Finns detaljerad information i [i den här artikeln][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Flera nätverkskort per VM
Du kan definiera flera virtuella nätverkskort (vNIC) för en virtuell dator i Azure. Möjlighet att ha flera vNICs kan du börja konfigurera nätverkstrafik dirigeras uppdelning där t.ex. klienttrafik skickas via ett virtuellt nätverkskort och backend-trafik via ett andra virtuellt nätverkskort. Beroende på vilken typ av VM har olika begränsningar för antalet vNICs angående. Exakta detaljer, funktioner och begränsningar finns i följande artiklar:

* [Skapa en virtuell dator med flera nätverkskort][virtual-networks-multiple-nics]
* [Distribuera flera NIC virtuella datorer med hjälp av en mall][virtual-network-deploy-multinic-arm-template]
* [Distribuera flera NIC virtuella datorer med hjälp av PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Distribuera flera NIC virtuella datorer med hjälp av Azure CLI][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Plats-till-plats-anslutning
Anslutningar mellan lokala är virtuella Azure-datorer och lokala kopplad till en transparent och permanenta VPN-anslutning. Det förväntas att bli vanligaste SAP-distribution mönster i Azure. Antas att operativa procedurer och processer med SAP-instanser i Azure bör fungera transparent. Detta betyder att du ska kunna skriva ut utanför dessa system som används SAP Transport Management System (Proportionerlig) till transport ändras från en utvecklingssystemet i Azure till ett testsystem som är distribuerade lokalt. Mer dokumentation runt plats-till-plats finns i [i den här artikeln][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-Tunnel-enhet
För att skapa en plats-till-plats-anslutning (lokalt Datacenter till Azure-datacenter), behöver du hämta och konfigurera en VPN-enhet eller använda Routing and Remote Access Service (RRAS) som introducerades som en programvarukomponent med Windows Server 2012.

* [Skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning med hjälp av PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Om VPN-enheter för plats-till-plats VPN-Gateway-anslutningar][vpn-gateway-about-vpn-devices]
* [Vanliga frågor och svar för VPN-Gateway][vpn-gateway-vpn-faq]

![Plats-till-plats-anslutning mellan Azure och lokalt][planning-guide-figure-600]

Ovanstående bild visar två Azure-prenumerationer har IP-adressen underintervall reserverade för användning i virtuella nätverk i Azure. Anslutningen från lokala nätverk till Azure upprättas via VPN.

#### <a name="point-to-site-vpn"></a>Punkt-till-plats VPN
Punkt-till-plats VPN kräver varje klientdator att ansluta till sin egen VPN till Azure. Punkt-till-plats-anslutning är inte praktiska för SAP-scenarier där vi tittar på. Därför får inga ytterligare referenser till punkt-till-plats VPN-anslutning.

[comment]: <> (MSSedusch – Mer information hittar du här)
[comment]: <> (MShermannd TODO-länken inte längre giltigt. Men ändå stöds inte ARM - se nästa länk nedan)
[comment]: <> (MSSedusch--< http://msdn.microsoft.com/library/azure/dn133798.aspx>.)
[comment]: <> (MShermannd TODO punkt till plats stöds inte ännu med ARM)
[comment]: <> (MSSedusch--< https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/>)

#### <a name="multi-site-vpn"></a>Flera plats-VPN
Azure erbjuder också Nuförtiden möjlighet att skapa flera plats-VPN-anslutning för en Azure-prenumeration. Tidigare var en enda prenumeration begränsat till en plats-till-plats VPN-anslutning. Den här begränsningen försvann med flera plats-VPN-anslutningar för en enda prenumeration. Detta gör det möjligt att använda mer än en Azure-Region för en viss prenumeration via anslutningar mellan lokala konfigurationer.

Mer dokumentation finns [i den här artikeln][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (MShermannd TODO hittades ingen ARM doku länk)

#### <a name="vnet-to-vnet-connection"></a>Virtuella nätverk på VNet-anslutning
Med hjälp av VPN för flera platser, måste du konfigurera ett separat virtuellt Azure-nätverk i var och en av regionerna. Du har dock ofta kravet att programkomponenterna i olika områden ska kommunicera med varandra. Den här kommunikationen ska helst inte dirigeras från en Azure till lokala och därifrån andra Azure-regionen. Azure erbjuder genvägen möjlighet att konfigurera en anslutning från ett virtuellt Azure-nätverk i en region till en annan Azure-nätverk finns i en annan region. Den här funktionen kallas VNet-till-VNet-anslutningen. Mer information om den här funktionen finns här: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure--expressroute"></a>Privat anslutning till Azure – ExpressRoute
Microsoft Azure ExpressRoute kan privata anslutningar mellan Azure-datacenter och antingen kundens lokala infrastruktur eller i en miljö med samplacering skapas. ExpressRoute erbjuds av olika MPLS (paketväxlat) VPN-leverantörer eller andra leverantörer. ExpressRoute-anslutningar går inte via offentligt Internet. ExpressRoute-anslutningar ger högre säkerhet och högre tillförlitlighet via flera parallella kretsar, högre hastighet och lägre svarstider än vanliga anslutningar över Internet.

Hitta mer information om Azure ExpressRoute och erbjudanden här:

* <https://Azure.microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-faqs/>

Expressroute gör att flera Azure-prenumerationer via en ExpressRoute-krets dokumenteras här

* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-Circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Tvingad tunneltrafik vid mellan platser
För virtuella datorer ansluta till lokala domäner via plats-till-plats, punkt-till-plats eller ExpressRoute, måste du se till att Internet-proxyinställningarna komma att distribueras för alla användare i dessa virtuella datorer samt. Som standard program som körs i de virtuella datorer eller användare som använder en webbläsare för att komma åt internet inte skulle gå igenom företagets proxy, men det skulle ansluta direkt till Azure till internet. Men även Proxyinställningen är inte en 100%-lösning för att dirigera trafik via företagets proxy eftersom det är ansvar för program och tjänster för att söka efter proxyn. Om program som körs i den virtuella datorn inte gör detta eller ändrar inställningarna för en administratör, trafik till Internet kan vara detoured igen direkt via Azure till Internet.

För att undvika detta, kan du konfigurera Tvingad Tunneling med plats-till-plats-anslutning mellan Azure och lokalt. Detaljerad beskrivning av funktionen Tvingad tunneltrafik publiceras här <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Tvingad Tunneling med ExpressRoute aktiveras av kunder annonserar en standardväg via ExpressRoute BGP-peeringsessioner.

#### <a name="summary-of-azure-networking"></a>Översikt över Azure-nätverk
Det här kapitlet innehåller många viktiga punkter om Azure-nätverk. Här följer en sammanfattning av de viktigaste aspekterna:

* Virtuella Azure-nätverk kan du konfigurera nätverket enligt dina behov
* Virtuella Azure-nätverk kan utnyttjas för att tilldela IP-adressintervall till virtuella datorer eller tilldela fasta IP-adresserna till virtuella datorer
* Om du vill konfigurera en plats-till-plats eller en punkt-till-plats-anslutning måste du skapa ett Azure Virtual Network först
* När en virtuell dator har distribuerats är det inte längre går att ändra det virtuella nätverket som tilldelats den virtuella datorn

### <a name="quotas-in-azure-virtual-machine-services"></a>Kvoter i virtuell dator i Azure-tjänster
Vi måste veta det faktum att lagring och nätverksinfrastruktur delas mellan virtuella datorer som kör en mängd olika tjänster i Azure-infrastrukturen. Och precis som för datacenter kundens egna överbelasta på några av infrastrukturresurser utförs i grad. Microsoft Azure-plattformen använder disk-, processor-, nätverks- och andra kvoter att begränsa de resurser som används och att bevara konsekvent och deterministisk prestanda.  Olika VM-typer (A5, A6 och så vidare) har olika kvoter för antalet diskar, processor, RAM-minne och nätverk.

> [!NOTE]
> Processor och minne resurser av VM-typer som stöds av SAP är förallokerade på värdnoderna. Detta innebär att när den virtuella datorn har distribuerats, resurser på värden är tillgänglig som definieras av VM-typen.
>
>

Kvoter för varje virtuell datorstorlek måste beaktas när du planerar och storleksanpassa SAP på Azure-lösningar.  VM-kvoter beskrivs [här][virtual-machines-sizes].

Kvoter beskrivs representerar teoretisk högsta värden.  Gränsen på IOPS per virtuell Hårddisk kan uppnås med små IOs (8kb), men de kan eventuellt inte uppnås med stora IOs (1Mb).  IOPS-gräns tillämpas på Granulariteten för enstaka virtuella hårddiskar.

Som en grov beslutsträd avgöra om ett SAP-system som passar till Azure-tjänster för virtuella datorer och dess funktioner, eller om ett befintligt system måste konfigureras på olika sätt för att distribuera system på Azure, kan du använda beslutsträdet nedan:

![Beslutsträd för att bestämma möjlighet att distribuera SAP på Azure][planning-guide-figure-700]

**Steg 1**: den viktigaste informationen är att börja med SAP-krav för ett visst SAP-system. SAP-krav måste delas in den DBMS och SAP programmet del, även om SAP-systemet är redan distribuerad lokalt i en nivå 2-konfiguration. SAP relaterade till maskinvaran som används ofta kan fastställa eller beräknat baserat på befintliga SAP prestandamått för befintliga system. Resultaten finns här: <http://global.sap.com/campaigns/benchmark/index.epx>.
För nyligen distribuerade SAP-system, bör du har gått igenom en storlek Övning som bestämmer SAP kraven i systemet.
Se även den här bloggen och bifogade dokumentet för SAP storlek på Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Steg 2**: för befintliga system i/o-volym och i/o-åtgärder per sekund på servern DBMS mäts. För nyligen planerad system bör sizing Övning för det nya systemet även ge grov uppslag av i/o-kraven på DBMS-sida. Om osäker, måste så småningom du genomföra ett konceptbevis.

**Steg 3**: jämföra SAP-krav för DBMS-server med SAP Azure VM olika kan ge. Informationen i de olika typerna av Azure VM SAP dokumenteras i SAP-kommentar [1928533]. Fokus bör finnas på DBMS VM först eftersom databasen lagret är lagret ett SAP NetWeaver system som inte kan skaländras ut i flesta distributioner. Däremot kan programnivå SAP skalas ut. Om ingen av SAP stöds Virtuella Azure-typer kan tillhandahålla nödvändiga SAP, arbetsbelastningen för det planerade SAP-systemet kan inte köras på Azure. Antingen måste du distribuera system lokalt eller måste du ändra arbetsbelastningen volymen för systemet.

**Steg 4**: enligt [här][virtual-machines-sizes], Azure tillämpar en kvot för IOPS per VHD oberoende om du använder standardlagring eller Premium-lagring. Beroende på vilken typ av VM, antalet virtuella hårddiskar som kan monteras varierar. Därför kan du beräkna IOPS maximalt som kan uppnås med var och en av de olika typerna av virtuell dator. Beroende på databasen filens layout, du kan stripe-VHD: er till en volym i gästoperativsystemet. Dock om den aktuella volymen IOPS för en distribuerad SAP-systemet överskrider de beräknade gränserna för den största VM-typ av Azure och om det finns ingen risk för att kompensera med mer minne, kan arbetsbelastningen för SAP-system påverkas negativt. I sådana fall måste nådde du en plats där du inte bör distribuera system på Azure.

**Steg 5**: särskilt SAP distribuerade system som är lokalt i nivå 2 konfigurationer finns risken att systemet kan behöva konfigureras på Azure i en nivå 3-konfiguration. I det här steget behöver du kontrollera om det finns en komponent i programnivå SAP som inte skalas ut och som inte passar in i processor och minne resurser som de olika typerna av Virtuella Azure erbjuder. Om det verkligen är en sådan komponent, kan inte SAP-systemet och dess arbetsbelastningen distribueras till Azure. Men om du kan skala ut SAP-programkomponenter i flera virtuella Azure-datorer, systemet kan distribueras till Azure.

**Steg 6**: om DBMS och SAP layer programkomponenter kan köras på virtuella Azure-datorer, konfigurationen måste definieras med avseende på:

* Antal virtuella Azure-datorer
* VM-typer för de enskilda komponenterna
* Antal virtuella hårddiskar i DBMS VM att ge tillräckligt med IOPS

## <a name="managing-azure-assets"></a>Hantera Azure tillgångar
### <a name="azure-portal"></a>Azure Portal
Azure Portal är en av tre gränssnitt för att hantera Virtuella Azure-distributioner. Grundläggande hanteringsuppgifter som att distribuera virtuella datorer från bilder, kan göras via Azure Portal. Skapande av Lagringskonton, virtuella nätverk och andra Azure-komponenter är dessutom också uppgifter som kan hantera mycket bra i Azure Portal. Men funktioner som överför virtuella hårddiskar från lokal till Azure eller kopierar en VHD i Azure är uppgifter som kräver verktyg från tredje part eller administration via PowerShell eller CLI.

![Microsoft Azure Portal – översikt över virtuella datorer][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Uppgifter för administration och konfiguration för den virtuella dator-instansen kan utföras från i Azure-portalen.

Förutom att starta om och stänga av en virtuell dator kan också koppla, koppla från och skapa datadiskar för att avbilda instansen för förberedelse av avbildningen och konfigurera storleken på den virtuella datorinstans instansen virtuella.

Azure Portal tillhandahåller grundläggande funktioner för att distribuera och konfigurera virtuella datorer och många andra Azure-tjänster. Men inte alla tillgängliga funktioner omfattas av Azure Portal. I Azure-portalen går inte att utföra åtgärder som:

* Överföring av virtuella hårddiskar till Azure
* Kopiera virtuella datorer

[comment]: <> (MShermannd TODO vad om automatisering i tjänsten för SAP virtuella datorer?)
[comment]: <> (MSSedusch distribution av flera virtuella datorer os under tiden möjligt)
[comment]: <> (MSSedusch också någon typ av automation om distributionen är inte möjligt med Azure-portalen. Det går inte att åtgärder som skript distribution av flera virtuella datorer via Azure Portal.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Hantering via Microsoft Azure PowerShell-cmdlets
Windows PowerShell är ett kraftfullt och utökningsbart ramverk som har varit brett bland kunder distribuera större mängder system i Azure. Efter installation av PowerShell-cmdlet på en stationär dator, bärbar dator eller dedikerade management station kan PowerShell-cmdlet: ar köras via fjärranslutning.

Processen för att aktivera en lokal desktop dator för användning av Azure PowerShell-cmdlets och hur du konfigurerar dem för användning med Azure-prenumeration(er) beskrivs i [i den här artikeln][powershell-install-configure].

Mer detaljerade instruktioner för hur du installerar, uppdatera och konfigurera Azure PowerShell cmdlets finns i [detta kapitel i distributionsguiden för][deployment-guide-4.1].

Customer experience har hittills att PowerShell (PS) verkligen är det mer kraftfulla verktyget att distribuera virtuella datorer och skapa anpassade steg i distributionen av virtuella datorer. Alla kunder SAP-instanser som körs i Azure använder PS-cmdlets för att komplettera hanteringsuppgifter de i Azure-portalen eller använder även PS-cmdlets exklusivt för att hantera sina distributioner i Azure. Eftersom Azure specifika cmdletar delar samma namngivningskonvention som mer än 2000 relaterade Windows-cmdletar, är en enkel åtgärd för Windows-administratörer utnyttja dessa cmdlets.

Finns det här exemplet: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd TODO Beskriver nya CLI-kommandot när testas)
Distribution av Azure övervakning tillägget för SAP (finns i kapitlet [Azure övervakning lösning för SAP] [ planning-guide-9.1] i det här dokumentet) är bara möjligt via PowerShell eller CLI. Därför är det obligatoriskt att installera och konfigurera PowerShell eller CLI när du distribuerar eller administrera en SAP NetWeaver system i Azure.  

Som Azure tillhandahåller fler funktioner, kommer nya PS-cmdlets som ska läggas till som kräver en uppdatering av cmdlets. Därför är det klokt att kontrollera webbplatsen Azure hämta minst en gång i månaden <https://azure.microsoft.com/downloads/> för en ny version av cmdlets. Den nya versionen installeras bara på den äldre versionen.

För en allmän lista över Azure med PowerShell-kommandon finns här: <https://msdn.microsoft.com/library/azure/dn708514.aspx>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Hantering via Microsoft Azure CLI-kommandona
För kunder som använder Linux och vill hantera Azure kanske resurser Powershell inte är ett alternativ. Microsoft erbjuder Azure CLI som ett alternativ.
Azure CLI tillhandahåller en uppsättning med öppen källkod, plattformsoberoende kommandon för att arbeta med Azure-plattformen. Azure CLI tillhandahåller i stort sett samma funktioner som finns i Azure-portalen.

Information om installation, konfiguration och hur du använder CLI finns-kommandon för att utföra uppgifter för Azure

* [Installera Azure CLI][xplat-cli]
* [Distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och Azure CLI][virtual-machines-linux-cli-deploy-templates]
* [Använda Azure CLI för Mac, Linux och Windows med Azure Resource Manager][xplat-cli-azure-resource-manager]

Läs även kapitel [Azure CLI för virtuella Linux-datorer] [ deployment-guide-4.5.2] i den [Deployment Guide] [ planning-guide] om hur du använder Azure CLI för att distribuera Azure-övervakning Tillägget för SAP.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Olika sätt att distribuera virtuella datorer för SAP i Azure
I det här kapitlet får du lära dig olika sätt att distribuera en virtuell dator i Azure. Förberedelse av ytterligare procedurer, samt hantering av virtuella hårddiskar och virtuella datorer i Azure beskrivs i detta kapitel.

### <a name="deployment-of-vms-for-sap"></a>Distribution av virtuella datorer för SAP
Microsoft Azure tillhandahåller flera olika sätt att distribuera virtuella datorer och associerade diskar. Därför är det mycket viktigt att förstå skillnaderna eftersom förberedelser för de virtuella datorerna kan skiljer sig åt beroende på vilken metod för distribution. I allmänhet ska vi ta en titt på följande scenarier:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Flytta en virtuell dator från lokal till Azure med en icke-generaliserad disk
Du planerar att flytta ett särskilt SAP-system från lokal till Azure. Detta kan göras genom att överföra den virtuella Hårddisken som innehåller binärfiler OS, SAP binärfiler och DBMS plus de virtuella hårddiskarna med data och loggfilen filer med DBMS till Azure. Jämfört med [scenario #2 nedan][planning-guide-5.1.2], du behåller värdnamn, SAP-SID och SAP användarkonton i Azure VM som de har konfigurerats i den lokala miljön. Därför är att generalisera bilden inte nödvändigt. Se kapitlen [förberedelse för att flytta en virtuell dator från lokal till Azure med en icke-generaliserad disk] [ planning-guide-5.2.1] i det här dokumentet för lokala förberedelsesteg och överföra ej generaliserad virtuella datorer eller virtuella hårddiskar att Azure. Läs kapitel [Scenario 3: flytta en virtuell dator från lokalt med hjälp av en ej generaliserad Azure VHD med SAP] [ deployment-guide-3.4] i den [Deployment Guide] [ deployment-guide]detaljerade anvisningar för att distribuera en sådan avbildning i Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Distribuera en virtuell dator med en viss kund-bild
På grund av särskilda krav för ditt operativsystem eller DBMS-version, kan de tillhandahållna avbildningarna i Azure Marketplace inte passar dina behov. Därför kan du behöva skapa en virtuell dator med hjälp av egna ”privat” OS/DBMS VM-avbildning som kan användas flera gånger efteråt. För att förbereda en ”privat” avbildning för duplicering, måste följande beaktas:


[comment]: <> (MSSedusch > finns mer information här:)
[comment]: <> (MShermannd TODO första länken är om klassiska modellen. Hittade inte en Azure doku artikel)
[comment]: <> (MSSedusch >< https://azure.microsoft.com/documentation/articles/virtual-machines-create-upload-vhd-windows-server/>)
[comment]: <> (MSSedusch >< http://blogs.technet.com/b/blainbar/archive/2014/09/12/modernizing-your-infrastructure-with-hybrid-cloud-using-custom-vm-images-and-resource-groups-in-microsoft-azure-part-21-blain-barton.aspx>)
- - -
> ![Windows][Logo_Windows] Windows
>
> Windows-inställningar (t.ex. Windows-SID och värdnamn) måste vara tas ut/generaliserad på den lokala virtuella datorn via kommandot sysprep.
>
>
> ![Linux][Logo_Linux] Linux
>
> Följ stegen som beskrivs i följande artiklar innehåller [SUSE] [ virtual-machines-linux-create-upload-vhd-suse] eller [Red Hat] [ virtual-machines-linux-redhat-create-upload-vhd] att förbereda en virtuell Hårddisk som skulle överföras till Azure.
>
>

- - -
Om du redan har installerat SAP innehållet i den lokala virtuella datorn (särskilt för nivå 2-system), kan du anpassa SAP systeminställningar när proceduren som stöds av SAP programvara etablering Manager distributionen av virtuella Azure-datorn via instansen (SAP-kommentar [1619720]). Se kapitlen [förberedelse för att distribuera en virtuell dator med en viss kund-bild för SAP] [ planning-guide-5.2.2] och [ladda upp en lokal VHD till Azure] [ planning-guide-5.3.2]i det här dokumentet för lokala förberedelser och överföring av en generaliserad virtuell dator till Azure. Läs kapitel [Scenario 2: distribuera en virtuell dator med en anpassad avbildning för SAP] [ deployment-guide-3.3] i den [Deployment Guide] [ deployment-guide] detaljerade anvisningar för distribuera en sådan avbildning i Azure.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Distribuera en virtuell dator utanför Azure Marketplace
Du vill använda en Microsoft eller 3 part angivna VM-avbildning från Azure Marketplace för att distribuera den virtuella datorn. När du har distribuerat den virtuella datorn i Azure måste följa du samma riktlinjer och verktyg för att installera program och/eller DBMS inuti den virtuella datorn som du gör i en lokal miljö. Mer detaljerad distributionsbeskrivning av finns kapitel [Scenario 1: distribuera en virtuell dator utanför Azure Marketplace för SAP] [ deployment-guide-3.2] i den [Deployment Guide][deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Förbereda virtuella datorer med SAP för Azure
Uppfylla vissa krav innan du laddar upp virtuella datorer till Azure måste du se till att virtuella datorer och virtuella hårddiskar. Det finns mindre skillnader beroende på vilken distributionsmetod som används.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Förberedelser för att flytta en virtuell dator från lokal till Azure med en icke-generaliserad disk
En vanlig distributionsmetod är att flytta en befintlig virtuell dator som kör ett SAP-system från lokal till Azure. Den virtuella datorn och SAP-system på den virtuella datorn ska precis köras i Azure med samma värdnamn och mycket sannolikt samma SAP-SID. I det här fallet bör gästen OS VM inte vara generaliserad för flera distributioner. Om det lokala nätverket har utökats till Azure (se kapitel [mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med krav på att fullständigt integrerat i det lokala nätverket] [ planning-guide-2.2] i det här dokumentet), även samma domänkonton kan användas i den virtuella datorn eftersom de användes innan lokalt.

Kraven när du förbereder din egen Azure VM-disken är:

* Den virtuella Hårddisken som innehåller operativsystemet gick ursprungligen har endast en maximal storlek på 127GB. Den här begränsningen har eliminerats i slutet av mars 2015. Den virtuella Hårddisken som innehåller operativsystemet kan nu vara upp till 1TB i storlek som VHD finns också i Azure Storage.

[comment]: <> (MShermannd TODO måste du kontrollera om CLI också konverteras till statisk)
* Den måste vara i formatet fast virtuell Hårddisk. Dynamiska virtuella hårddiskar eller virtuella hårddiskar i VHDx-format stöds inte ännu i Azure. Dynamiska virtuella hårddiskar ska konverteras till statisk virtuella hårddiskar när du överför den virtuella Hårddisken med PowerShell-kommandon eller CLI
* Virtuella hårddiskar som är monterade på den virtuella datorn och ska monteras igen i Azure VM måste vara i ett fast VHD-format samt. Samma storleksgränsen på OS-disken gäller för datadiskar. Virtuella hårddiskar kan ha en maximal storlek på 1TB. Dynamiska virtuella hårddiskar ska konverteras till statisk virtuella hårddiskar när du överför den virtuella Hårddisken med PowerShell-kommandon eller CLI
* Lägg till en annan lokalt konto med administratörsbehörighet som kan användas av Microsoft-supporten eller som kan tilldelas som kontext för tjänster och program att köras tills den virtuella datorn har distribuerats och mer lämpliga användare kan användas.
* Om du använder en molnbaserad distributionsscenariot (finns i kapitlet [endast molnbaserad - distributioner på virtuella datorer till Azure utan beroenden i lokala kunden nätverket] [ planning-guide-2.1] i det här dokumentet) i kombination med den här distributionsmetoden domänkonton kanske inte fungerar när Azure-disken har distribuerats i Azure. Detta gäller särskilt för konton som används för att köra tjänster som DBMS eller SAP-program. Därför måste du ersätta dessa domänkonton med VM lokala konton och ta bort domänkonton lokalt på den virtuella datorn. Behålla lokalt domänanvändare i VM-avbildning är inte ett problem när den virtuella datorn distribueras i scenarion med flera lokaler som beskrivs i kapitlet [mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med kravet att fullständigt integrerat i det lokala nätverket] [ planning-guide-2.2] i det här dokumentet.
* Om domänkonton användes som DBMS-inloggningar eller användare kör system lokal och dessa virtuella datorer som är avsedda för att distribueras i endast molnbaserad scenarier, måste användarna tas bort. Du måste se till att den lokala administratören plus en annan VM lokal användare läggs till som en inloggning till DBMS som administratörer.
* Lägg till andra lokala konton som de kan behövas för det specifika distributionsscenariot.

- - -
> ![Windows][Logo_Windows] Windows
>
> I det här scenariot krävs inga generalisering (sysprep) för den virtuella datorn för att ladda upp och distribuera den virtuella datorn på Azure.
> Se till att enheten inte är D:\ användas Set disk automount för anslutna diskar som beskrivs i kapitlet [inställningen automount för anslutna diskar] [ planning-guide-5.5.3] i det här dokumentet.
>
> ![Linux][Logo_Linux] Linux
>
> I det här scenariot inga generalisering (waagent-deprovision) för den virtuella datorn krävs för att ladda upp och distribuera den virtuella datorn på Azure.
> Kontrollera att/mnt/resurs inte används och att alla diskar är monterade via uuid. Se till att posten startprogrammet också avspeglar uuid-baserade montering för OS-disk.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Förberedelser för att distribuera en virtuell dator med en viss kund-bild för SAP
VHD-filer som innehåller en generaliserad OS lagras också i behållare på Azure Storage-konton. Du kan distribuera en ny virtuell dator från dessa avbildningar VHD genom att referera till den virtuella Hårddisken som en källa VHD i din distribution mallfilerna som beskrivs i kapitlet [Scenario 2: distribuera en virtuell dator med en anpassad avbildning för SAP] [ deployment-guide-3.3] av [ Distributionsguide för][deployment-guide].

Kraven när du förbereder din egen Azure VM-avbildning är:

* Den virtuella Hårddisken som innehåller operativsystemet gick ursprungligen har endast en maximal storlek på 127GB. Den här begränsningen har eliminerats i slutet av mars 2015. Den virtuella Hårddisken som innehåller operativsystemet kan nu vara upp till 1TB i storlek som VHD finns också i Azure Storage.

[comment]: <> (MShermannd TODO måste du kontrollera om CLI också konverteras till statisk)
* Den måste vara i formatet fast virtuell Hårddisk. Dynamiska virtuella hårddiskar eller virtuella hårddiskar i VHDx-format stöds inte ännu i Azure. Dynamiska virtuella hårddiskar ska konverteras till statisk virtuella hårddiskar när du överför den virtuella Hårddisken med PowerShell-kommandon eller CLI
* Virtuella hårddiskar som är monterade på den virtuella datorn och ska monteras igen i Azure VM måste vara i ett fast VHD-format samt. Samma storleksgränsen på OS-disken gäller för datadiskar. Virtuella hårddiskar kan ha en maximal storlek på 1TB. Dynamiska virtuella hårddiskar ska konverteras till statisk virtuella hårddiskar när du överför den virtuella Hårddisken med PowerShell-kommandon eller CLI
* Eftersom alla domänanvändare registrerad som användare i den virtuella datorn inte finns i ett scenario med endast molnbaserad (finns i kapitlet [endast molnbaserad - distributioner på virtuella datorer till Azure utan beroenden i lokala kunden nätverket] [ planning-guide-2.1] i det här dokumentet), tjänster som använder dessa konton inte kanske fungerar när avbildningen har distribuerats i Azure-domän. Detta gäller särskilt för konton som används för att köra tjänster som DBMS eller SAP-program. Därför måste du ersätta dessa domänkonton med VM lokala konton och ta bort domänkonton lokalt på den virtuella datorn. Behålla lokalt domänanvändare i VM-avbildning kanske inte är ett problem när den virtuella datorn distribueras i scenarion med flera lokaler som beskrivs i kapitlet [mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med kravet att fullständigt integrerat i det lokala nätverket] [ planning-guide-2.2] i det här dokumentet.
* Lägg till en annan lokalt konto med administratörsbehörighet som kan användas av Microsoft-supporten i problemet undersökningar eller som kan tilldelas som kontext för tjänster och program att köras tills den virtuella datorn har distribuerats och mer lämpliga användare kan användas.
* I distributioner för endast molnbaserad och där domänkonton användes som DBMS-inloggningar eller användare när du kör system lokal, bör användarna tas bort. Du måste se till att den lokala administratören plus en annan VM lokal användare läggs till som en inloggning/användare av DBMS som administratörer.
* Lägg till andra lokala konton som de kan behövas för det specifika distributionsscenariot.
* Om bilden innehåller en installation av SAP NetWeaver och namnbyte av du värdnamnet från det ursprungliga namnet på Azure-distribution är antagligen det rekommenderas att kopiera de senaste versionerna av SAP programvara etablering Manager DVD i mallen. Detta gör att du enkelt använda SAP tillhandahålls Byt namn på funktionen för att anpassa ändrade värdnamnet och/eller ändra SID för SAP-system inom distribuerade VM-avbildning som en ny kopia har startats.

- - -
> ![Windows][Logo_Windows] Windows
>
> Se till att enheten inte är D:\ användas Set disk automount för anslutna diskar som beskrivs i kapitlet [inställningen automount för anslutna diskar] [ planning-guide-5.5.3] i det här dokumentet.
>
> ![Linux][Logo_Linux] Linux
>
> Kontrollera att/mnt/resurs inte används och att alla diskar är monterade via uuid. För att kontrollera att den OS-disk visar startprogrammet posten också montera uuid-baserade.
>
>

- - -
* SAP GUI (för administrativa och konfigurera ändamål) kan förinstalleras i denna mall.
* Annan programvara som krävs för att köra de virtuella datorerna i mellan lokala scenarier kan installeras så länge som det här programmet kan arbeta med det nya namnet på den virtuella datorn.

Om den virtuella datorn är tillräckligt beredd att vara generisk och kan slutligen oberoende av konton/användare inte tillgänglig i det aktuella Azure distributionsscenariot, utförs senaste förberedelsen för att generalisera sådana en bild.

##### <a name="generalizing-a-vm"></a>Att generalisera en virtuell dator
- - -
[comment]: <> (MShermannd TODO måste hitta bättre artiklar / doku om generalisera VMs för ARM)
> ![Windows][Logo_Windows] Windows
>
> Det sista steget är att logga in på en virtuell dator med ett administratörskonto. Öppna en Windows-kommandofönster som ”administratör”. Gå till ...\windows\system32\sysprep och köra sysprep.exe.
> Ett litet fönster visas. Det är viktigt att kontrollera alternativet 'Generalize ”(standardvärdet är inte markerad) och ändra avstängning från standard 'Omstart' till 'Avstängning'. Den här proceduren förutsätter att sysprep-processen körs lokalt i Gästoperativsystem på en virtuell dator.
> Om du vill utföra proceduren med en virtuell dator som redan körs i Azure, följer du stegen som beskrivs i [i den här artikeln][virtual-machines-windows-capture-image].
>
> ![Linux][Logo_Linux] Linux
>
> [Så här skapar du en virtuell Linux-dator kan användas som en Resource Manager-mall][virtual-machines-linux-capture-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Överföring av virtuella datorer och virtuella hårddiskar mellan lokala till Azure
Eftersom det inte går att överföra VM-avbildningar och diskar till Azure via Azure Portal, behöver du använda Azure PowerShell-cmdlets eller CLI. En annan möjlighet är att använda verktyget 'AzCopy'. Verktyget kan kopiera virtuella hårddiskar mellan lokala och Azure (i båda riktningarna). Det kan också kopiera virtuella hårddiskar mellan Azure-regioner. Kontakta [denna dokumentation] [ storage-use-azcopy] om hämtning och användning av AzCopy.

Ett tredje alternativ är att använda olika inriktade GUI-verktyg från tredje part. Dock se till att verktygen stöder Azure Sidblobar. Vi behöver använda Azure sidan Blob store för våra ändamål (skillnaderna beskrivs här: <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>). Azure-verktygen är också mycket effektivt för komprimering av virtuella datorer och virtuella hårddiskar som behöver överföras. Detta är viktigt eftersom den här effektiviteten i komprimering minskar den tid (som varierar ändå beroende på länken överföringen till internet från den lokala anläggningen och distribution av Azure-region som mål). Det är en verkligt antagandet att överföra en virtuell dator eller en VHD från Europeiska plats till USA baserat Azure data Datacenter tar längre tid än överföring av samma virtuella datorer/virtuella hårddiskar till Europeiska Azure-datacenter.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Ladda upp en lokal VHD till Azure
För att överföra en befintlig virtuell dator eller en VHD från det lokala nätverket sådan VM eller VHD måste uppfylla kraven som anges i kapitel [förberedelse för att flytta en virtuell dator från lokal till Azure med en icke-generaliserad disk] [ planning-guide-5.2.1] i det här dokumentet.

Sådana en virtuell dator behöver inte vara generaliserad och kan överföras i tillstånd och formen som den har efter avstängning på den lokala sidan. Detsamma gäller för ytterligare virtuella hårddiskar som inte innehåller något operativsystem.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Överför en virtuell Hårddisk och gör det Azure-disken
I det här fallet vill vi överföra en virtuell Hårddisk, med eller utan ett operativsystem, och montera den till en virtuell dator som en datadisk eller använda den som OS-disken. Detta är en process med flera steg

**PowerShell**

* Logga in till din prenumeration med *Login-AzureRmAccount*
* Ställ in prenumerationen för din kontext med *Set-AzureRmContext* och parametern prenumerations-ID eller SubscriptionName - Se <https://msdn.microsoft.com/library/mt619263.aspx>
* Överför den virtuella Hårddisken med *Lägg till AzureRmVhd* till ett Azure Storage-konto - finns <https://msdn.microsoft.com/library/mt603554.aspx>
* Ange OS-disk på en ny VM-konfigurationen på den virtuella Hårddisken med *Set AzureRmVMOSDisk* -finns <https://msdn.microsoft.com/library/mt603746.aspx>
* Skapa en ny virtuell dator från VM-konfiguration med *ny AzureRmVM* -finns <https://msdn.microsoft.com/library/mt603754.aspx>
* Lägg till en datadisk till en ny virtuell dator med *Lägg till AzureRmVMDataDisk* -finns <https://msdn.microsoft.com/library/mt603673.aspx>

**Azure CLI**

* Växla till läget Azure Resource Manager med *azure config mode arm*
* Logga in till din prenumeration med *azure-inloggning*
* Välj din prenumeration med *azure-konto`<subscription name or id`>*
* Överför den virtuella Hårddisken med *azure storage blob överför* -finns [med hjälp av Azure CLI med Azure Storage][storage-azure-cli]
* Skapa en ny virtuell dator att ange den överförda virtuella Hårddisken som OS-disken med *azure vm skapa* och parametern -d
* Lägg till en datadisk till en ny virtuell dator med *vm disk bifoga nya*

**Mall**

* Överför den virtuella Hårddisken med Powershell eller Azure CLI
* Distribuera den virtuella datorn med en JSON-mall som refererar till den virtuella Hårddisken som visas i [exempel JSON mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-from-specialized-vhd/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Distribution av en VM-avbildning
För att överföra en befintlig virtuell dator eller en VHD från det lokala nätverket för att kunna använda den som en virtuell dator i Azure-avbildning sådan VM eller VHD måste uppfylla de krav som anges i kapitlet [förberedelse för att distribuera en virtuell dator med en viss kund-bild för SAP] [ planning-guide-5.2.2] i det här dokumentet.

* Använd *sysprep* i Windows eller *waagent-deprovision* på Linux för att generalisera den virtuella datorn – Se [så här skapar du en Windows-dator i Resource Manager-distributionsmodellen] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] eller [så här skapar du en virtuell Linux-dator kan användas som en Resource Manager-mall][virtual-machines-linux-capture-image-capture]
* Logga in till din prenumeration med *Login-AzureRmAccount*
* Ställ in prenumerationen för din kontext med *Set-AzureRmContext* och parametern prenumerations-ID eller SubscriptionName - Se <https://msdn.microsoft.com/library/mt619263.aspx>
* Överför den virtuella Hårddisken med *Lägg till AzureRmVhd* till ett Azure Storage-konto - finns <https://msdn.microsoft.com/library/mt603554.aspx>
* Ange OS-disk på en ny VM-konfigurationen på den virtuella Hårddisken med *Set-AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -finns <https://msdn.microsoft.com/library/mt603746.aspx>
* Skapa en ny virtuell dator från VM-konfiguration med *ny AzureRmVM* -finns <https://msdn.microsoft.com/library/mt603754.aspx>

**Azure CLI**

* Använd *sysprep* i Windows eller *waagent-deprovision* på Linux för att generalisera den virtuella datorn – Se [så här skapar du en Windows-dator i Resource Manager-distributionsmodellen] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] eller [så här skapar du en virtuell Linux-dator kan användas som en Resource Manager-mall] [ virtual-machines-linux-capture-image-capture] för Linux
* Växla till läget Azure Resource Manager med *azure config mode arm*
* Logga in till din prenumeration med *azure-inloggning*
* Välj din prenumeration med *azure-konto`<subscription name or id`>*
* Överför den virtuella Hårddisken med *azure storage blob överför* -finns [med hjälp av Azure CLI med Azure Storage][storage-azure-cli]
* Skapa en ny virtuell dator att ange den överförda virtuella Hårddisken som OS-disken med *azure vm skapa* och parametern -Q

**Mall**

* Använd *sysprep* i Windows eller *waagent-deprovision* på Linux för att generalisera den virtuella datorn – Se [så här skapar du en Windows-dator i Resource Manager-distributionsmodellen] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] eller [så här skapar du en virtuell Linux-dator kan användas som en Resource Manager-mall] [ virtual-machines-linux-capture-image-capture] för Linux
* Överför den virtuella Hårddisken med Powershell eller Azure CLI
* Distribuera den virtuella datorn med en JSON-mall som refererar till avbildningen VHD enligt [exempel JSON mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-to-on-premises"></a>Hämta virtuella hårddiskar till lokalt
Azure-infrastrukturen som en tjänst är inte en enkelriktad gata bara att kunna överföra virtuella hårddiskar och SAP system. Du kan flytta SAP-system från Azure tillbaka till lokala världen samt.

De virtuella hårddiskarna får inte vara aktiv under tid då nedladdningen. Även om hämtning av virtuella hårddiskar som är monterade till virtuella datorer, måste den virtuella datorn stängas. Om du vill hämta innehåll som ska användas för att ställa in ett nytt system lokalt och om du accepterar som under tiden för hämtningen och installationen av det nya systemet att systemet i Azure kan fortfarande vara operativa databasen , du kan undvika långa driftstopp genom att utföra en säkerhetskopiering av komprimerade databasen till en virtuell Hårddisk och hämta endast den virtuella Hårddisken i stället för hämtar grundläggande VM OS.

#### <a name="powershell"></a>PowerShell
När SAP-systemet har stoppats och den virtuella datorn är avstängd, kan du använda PowerShell-cmdleten spara AzureRmVhd på det lokala målet för att hämta VHD-diskarna tillbaka till lokala världen. För att kunna göra det, måste URL: en för den virtuella Hårddisken som du hittar i behöver 'Lagring avsnittet' i Azure-portalen (behovet av att navigera till Lagringskontot och lagringsbehållaren där den virtuella Hårddisken har skapats) och du veta var den virtuella Hårddisken ska kopieras till.

Du kan sedan använda kommandot genom att helt enkelt definiera parametern SourceUri som en URL för den virtuella Hårddisken ska ladda ned och LocalFilePath som den fysiska platsen för den virtuella Hårddisken (inklusive filnamnet). Kommandot kan se ut så:

```powerhell
Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
```

Mer information om cmdleten spara AzureRmVhd Kontrollera här <https://msdn.microsoft.com/library/mt622705.aspx>.

#### <a name="cli"></a>CLI
När SAP-systemet har stoppats och den virtuella datorn har stängts kan använda du Azure CLI kommandot azure storage blob nedladdningen på det lokala målet för att hämta VHD-diskar tillbaka till lokala världen. För att kunna göra det, måste namnet och en behållare för den virtuella Hårddisken som du kan hitta i 'lagring avsnittet' i Azure-portalen (behovet av att navigera till Lagringskontot och lagringsbehållaren där den virtuella Hårddisken har skapats) och du behöver veta var den virtuella Hårddisken ska copi utgåvan till.

Du kan sedan använda kommandot genom att helt enkelt definiera parametrar blob och behållare för den virtuella Hårddisken att ladda ned och mål som fysiska platsen för den virtuella Hårddisken (inklusive filnamnet). Kommandot kan se ut så:

```
azure storage blob download --blob <name of the VHD to download> --container <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --destination <destination of the VHD to download>
```

### <a name="transferring-vms-and-vhds-within-azure"></a>Överföring av virtuella datorer och virtuella hårddiskar i Azure
#### <a name="copying-sap-systems-within-azure"></a>Kopiera SAP-system i Azure
Troligtvis består en SAP-system eller även en dedikerad DBMS server som stöder ett lager för SAP-program av flera virtuella hårddiskar som innehåller antingen OS med binärfiler eller data och loggfilen fil(er) SAP-databasen. Varken funktionen Azure för att kopiera virtuella hårddiskar eller funktionen Azure för att spara virtuella hårddiskar till disk har en synkroniseringsmekanism som skulle ögonblicksbild flera virtuella hårddiskar synkront. Därför skulle tillståndet för de virtuella hårddiskarna kopieras eller har sparats även om de är monterade mot av samma virtuella dator vara olika. Detta innebär att det konkreta gäller att ha olika data och logfile(s) finns i olika virtuella hårddiskar, databasen i slutet skulle vara inkonsekvent.

**Slutsats: För att kunna kopiera eller spara virtuella hårddiskar som är en del av en SAP konfiguration du behöver stoppa SAP-systemet och också behöver stänga av den distribuerade virtuella datorn. Först då kan du kopiera eller ladda ned uppsättningen av virtuella hårddiskar att antingen skapa en kopia av SAP-system i Azure eller lokalt.**

Datadiskar lagras som VHD-filer i ett Azure Storage-konto och kan vara direkt ansluta till en virtuell dator eller användas som en bild. I det här fallet kopieras den virtuella Hårddisken till en annan plats innan beeing kopplade till den virtuella datorn. Det fullständiga namnet på VHD-filen i Azure måste vara unikt i Azure. Som nämnts tidigare redan är den typ av en tre delar namnet som ser ut som:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

##### <a name="powershell"></a>PowerShell
Du kan använda Azure PowerShell-cmdlets för att kopiera en VHD som visas i [i den här artikeln][storage-powershell-guide-full-copy-vhd].

##### <a name="cli"></a>CLI
Du kan använda Azure CLI för att kopiera en VHD som visas i [i den här artikeln][storage-azure-cli-copy-blobs]

##### <a name="azure-storage-tools"></a>Azure Storage-verktyg
* <http://azurestorageexplorer.Codeplex.com/releases/View/125870>

Det finns också tekniker utgåvor av Azure lagringsutforskare som finns här:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

Kopia av en VHD: n i ett lagringskonto är en process som tar endast några sekunder (ungefär som att skapa ögonblicksbilder med lazy kopia och kopiera vid skrivning till SAN-maskinvaran). När du har en kopia av VHD-filen som du kan koppla den till en virtuell dator eller använda den som en bild för att bifoga kopior av den virtuella Hårddisken till virtuella datorer.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption fromImage
$vm | Update-AzureRmVM
```
##### <a name="cli"></a>CLI
```
azure config mode arm

# attach a vhd to a vm
azure vm disk attach <resource group name> <vm name> <path to vhd>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Kopiera diskar mellan Azure Storage-konton
Den här uppgiften kan inte utföras på Azure Portal. Du kan ise Azure PowerShell-cmdlet: ar, Azure CLI eller tredje part lagring webbläsare. PowerShell-cmdletar eller CLI-kommandon kan skapa och hantera blobbar som omfattar möjligheten att kopiera BLOB asynkront mellan Lagringskonton och mellan regioner inom Azure-prenumerationen.

##### <a name="powershell"></a>PowerShell
Det är också möjligt att kopiera virtuella hårddiskar mellan prenumerationer. Mer information finns [i den här artikeln][storage-powershell-guide-full-copy-vhd].

Det grundläggande flödet av PS cmdlet logiken ser ut så här:

* Skapa en kontexten för lagringskontot för käll-lagringskonto med *ny AzureStorageContext* -finns <https://msdn.microsoft.com/library/dn806380.aspx>
* Skapa en kontexten för lagringskontot för mål-lagringskontot med *ny AzureStorageContext* -finns <https://msdn.microsoft.com/library/dn806380.aspx>
* Starta kopieringen med

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Kontrollera status för kopian i en slinga med

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Anslut den nya virtuella Hårddisken till en virtuell dator enligt beskrivningen ovan.

Exemplen finns [i den här artikeln][storage-powershell-guide-full-copy-vhd]

##### <a name="cli"></a>CLI
* Starta kopieringen med

```
  azure storage blob copy start --source-blob <source blob name> --source-container <source container name> --account-name <source storage account name> --account-key <source storage account key> --dest-container <target container name> --dest-blob <target blob name> --dest-account-name <target storage account name> --dest-account-key <target storage account name>
```

* Kontrollera status om kopian i en slinga med

```
azure storage blob copy show --blob <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Anslut den nya virtuella Hårddisken till en virtuell dator enligt beskrivningen ovan.

Exemplen finns [i den här artikeln][storage-azure-cli-copy-blobs]

### <a name="disk-handling"></a>Disk-hantering
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>VM/VHD-strukturen för SAP-distributioner
Vi är hanteringen av strukturen för en virtuell dator och associerade virtuella hårddiskar mycket enkelt. Kunder utvecklat många sätt att strukturera en serverinstallation lokala installationer.

* En bas-VHD som innehåller Operativsystemet och alla binärfiler DBMS och/eller SAP. Den här virtuella Hårddisken kan vara upp till 1TB i storlek i stället för tidigare begränsningar som begränsad till 127 GB sedan mars 2015.
* En eller flera virtuella hårddiskar som innehåller DBMS-loggfilen för SAP-databasen och loggfilen områdets DBMS tillfällig lagring (om DBMS stöder detta). Om databasen loggen IOPS kraven är hög, måste du stripe-flera virtuella hårddiskar för att uppnå den nödvändiga IOPS volymen.
* Ett antal virtuella hårddiskar som innehåller en eller två databasfiler för SAP-databasen och DBMS temp datafilerna samt (om DBMS stöder detta).

![För referenskonfiguration av Azure IaaS-VM för SAP][planning-guide-figure-1300]

[comment]: <> (MShermannd TODO beskrivs Linux struktur)

- - -
> ![Windows][Logo_Windows] Windows
>
> Med många kunder såg vi konfigurationer, till exempel SAP och DBMS binärfiler har inte installerats på enhet c:\ där Operativsystemet har installerats. Det finns olika orsaker, men när vi tillbaka till roten, var vanligtvis att enheterna har liten och OS uppgraderingar behövs ytterligare utrymme 10 – 15 år sedan. Båda villkoren gäller inte för ofta längre dessa dagar. Idag mappas enhet c:\ diskar med stora volymer eller på virtuella datorer. För att kunna hålla distributioner enkel i sin struktur, bör följa följande distribution för SAP NetWeaver system i Azure
>
> Windows-operativsystemet växlingsfilens storlek bör vara på d: (icke-beständig disk)
>
> ![Linux][Logo_Linux] Linux
>
> Placera Linux växlingsfil under /mnt/mnt/resurs på Linux som beskrivs i [i den här artikeln][virtual-machines-linux-agent-user-guide]. Växlingsfilen kan konfigureras i konfigurationsfilen för Linux-agenten /etc/waagent.conf. Lägg till eller ändra följande inställningar:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Om du vill aktivera ändringarna som måste du starta om Linux-agenten med

```
sudo service waagent restart
```

Läs SAP-kommentar [1597355] för mer information om rekommenderade växlingen filstorlek

- - -
Antalet virtuella hårddiskar som används för DBMS-datafiler och vilken typ av Azure Storage dessa virtuella hårddiskar finns på fastställas av IOPS-krav och svarstiden krävs. Exakt kvoter beskrivs i [i den här artikeln][virtual-machines-sizes]

Erfarenhet av SAP distributioner under de senaste 2 åren vilka undervisning förekommer oss vissa erfarenheter som kan sammanfattas som:

* IOPS trafik till olika datafiler är inte alltid samma eftersom befintliga kunden system kan ha annorlunda storlek datafiler som motsvarar deras SAP-databaserna. Därför visade det sig att bättre för att använda en RAID-konfiguration över flera virtuella hårddiskar för att placera data LUN högg utanför de. Det fanns situationer, särskilt med Azure standardlagring där en IOPS frekvens uppnått kvoten av en enskild virtuell Hårddisk mot DBMS transaktionsloggen. I sådana fall rekommenderas användning av Premium-lagring eller också sammanställa flera Standard lagring virtuella hårddiskar med en programvarubaserad RAID.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [Bästa praxis för SQL Server i Azure Virtual Machines prestanda][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Konfigurera programvarubaserad RAID på Linux][virtual-machines-linux-configure-raid]
> * [Konfigurera LVM på en virtuell Linux-dator i Azure][virtual-machines-linux-configure-lvm]
> * [Azure Storage hemligheter och Linux-i/o-optimeringar](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* Premium-lagring visas betydande bättre prestanda, särskilt för kritiska transaction log skrivningar. För SAP-scenarier som förväntas att leverera produktionsliknande prestanda, rekommenderas att använda VM-serien som kan använda Azure Premium-lagring.

Tänk på att den virtuella Hårddisken som innehåller Operativsystemet, och som vi rekommenderar om binärfilerna för SAP och databasen (grundläggande VM), är inte längre begränsad till 127GB. Det kan nu ha upp till 1TB i storlek. Detta bör vara tillräckligt med utrymme för att behålla alla nödvändiga fil som t.ex. inklusive SAP batch jobbloggar.

Fler förslag och mer specifikt för DBMS virtuella datorer, finns det [DBMS Deployment Guide][dbms-guide]

#### <a name="disk-handling"></a>Disk-hantering
I de flesta fall måste du skapa ytterligare diskar för att distribuera SAP-databasen till den virtuella datorn. Hittills har diskuterats överväganden för antalet virtuella hårddiskar i kapitlet [VM/VHD-strukturen för SAP distributioner] [ planning-guide-5.5.1] i det här dokumentet. Azure-portalen kan anslutning och frånkoppling diskar när en grundläggande virtuell dator har distribuerats. Diskarna kan vara kopplad/borttagbar när den virtuella datorn är igång och körs samt när den har stoppats. När du ansluter en disk, erbjuder Azure Portal bifoga en tom disk eller en befintlig disk som vid denna tidpunkt inte är ansluten till en annan virtuell dator.

**Obs**: virtuella hårddiskar kan endast kopplas till en virtuell dator samtidigt.

![Koppla / frånkoppla diskarna med Azure standardlagring][planning-guide-figure-1400]

Du måste bestämma om du vill skapa en ny och tom virtuell Hårddisk (som skulle skapas i samma Lagringskonto som den virtuella datorn är i bas) eller om du vill välja en befintlig virtuell Hårddisk som har överförts tidigare och som ska kopplas till den virtuella datorn nu.

**VIKTIGA**: du **inte** vill använda värden cachelagring med Azure standardlagring. Värden Cache-inställningar bör du lämna standardinställningen NONE. Med Azure Premium Storage bör du aktivera Läs cachelagring om i/o-egenskap är främst läsa som vanliga i/o-trafik mot databasen datafiler. Om databasen transaktionsloggfilen rekommenderas ingen cachelagring.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Så här kopplar du en datadisk i Azure-portalen][virtual-machines-windows-attach-disk-portal]
>
> Om diskar har anslutits måste du logga in till den virtuella datorn att öppna Windows Disk Manager. Om automount inte aktiveras som rekommenderas i kapitel [inställningen automount för anslutna diskar][planning-guide-5.5.3], just kopplat volymen behöver vidta online och initierade.
>
> ![Linux][Logo_Linux] Linux
>
> Om diskarna är anslutna, måste du logga in till den virtuella datorn och initiera diskarna som beskrivs i [i den här artikeln][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Om den nya disken är en tom disk, måste du formatera disken samt. Formatering, särskilt för DBMS data och loggfiler tillämpas samma rekommendationer för bare metal-distributioner av DBMS.

Som nämnts i kapitlet [i Microsoft Azure virtuella konceptet][planning-guide-3.2], ett Azure Storage-konto tillhandahåller inte oändlig resurser i i/o-volym IOPS och data volym. DBMS virtuella datorer som vanligtvis mest påverkas av detta. Det kan vara bäst att använda ett separat Lagringskonto för varje virtuell dator om du har några hög i/o-volym virtuella datorer som du distribuerar för att hålla sig inom gränsen på volymen Azure Storage-konto. I annat fall måste du se hur du balanserar dessa virtuella datorer mellan olika Storage-konton utan att gränsen på varje enskild Storage-konto. Mer information beskrivs i den [DBMS Deployment Guide][dbms-guide]. Du bör också tänka dessa begränsningar på för ren SAP-program virtuella server-datorer eller andra virtuella datorer som eventuellt kan kräva ytterligare virtuella hårddiskar.

Ett annat ämne som är relevant för Storage-konton är om de virtuella hårddiskarna i ett Lagringskonto får georeplikerad. GEO-replikering är aktiverat eller inaktiverat på nivån Storage-konto och inte på VM-nivå. Om geo-replikering är aktiverat, skulle de virtuella hårddiskarna inom Lagringskonto replikeras till en annan Azure-datacenter inom samma region. Innan du bestämmer dig för detta, bör du tänka på följande begränsningar:

Azure Geo-replikering fungerar lokalt på varje virtuell Hårddisk på en virtuell dator och replikerar inte IOs i kronologisk ordning över flera virtuella hårddiskar på en virtuell dator. Därför kan replikeras den virtuella Hårddisken som representerar den grundläggande VM samt de ytterligare virtuella hårddiskar anslutna till den virtuella datorn oberoende av varandra. Det innebär att det finns ingen synkronisering mellan ändringar i de olika virtuella hårddiskarna. Det faktum att IOs replikeras oberoende av den ordning som de är skrivna innebär att geo-replikering är inte av värdet för databasservrar som har sina databaser fördelade över flera virtuella hårddiskar. Förutom DBMS finnas det också andra program där processer skriva eller ändra data i olika virtuella hårddiskar och där det är viktigt att hålla ordning på ändringar. Om detta är ett krav, bör inte geo-replikering i Azure aktiveras. Beroende på om du behöver eller vill geo-replikering för en uppsättning virtuella datorer, men inte för en annan uppsättning, du kan redan kategorisera virtuella datorer och deras relaterade virtuella hårddiskar i olika Storage-konton som har geo-replikering aktiverat eller inaktiverat.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Ange automount för anslutna diskar
- - -
> ![Windows][Logo_Windows] Windows
>
> För virtuella datorer som har skapats från egna avbildningar eller diskar, är det nödvändigt att kontrollera och eventuellt ange parametern automount. Den här parametern kan den virtuella datorn efter en omstart eller Omdistributionen i Azure för att montera kopplad/monterade enheter igen automatiskt.
> Parametern har angetts för bilder som tillhandahålls av Microsoft i Azure Marketplace.
>
> Kontrollera i dokumentationen för den kommandorad körbara diskpart.exe här för att ange automount:
>
> * [Kommandoradsalternativ för DiskPart](https://technet.microsoft.com/library/cc766465.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
>
> Ska öppnas fönstret Windows-kommandoraden som administratör.
>
> Om diskar har anslutits måste du logga in till den virtuella datorn att öppna Windows Disk Manager. Om automount inte aktiveras som rekommenderas i kapitel [inställningen automount för anslutna diskar][planning-guide-5.5.3], nyligen kopplade volym > måste utföras online och initierade.
>
> ![Linux][Logo_Linux] Linux
>
> Du måste initiera en tom disk som nyss bifogad enligt beskrivningen i [i den här artikeln][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Du måste också lägga till nya diskar på /etc/fstab.
>
>

- - -
### <a name="final-deployment"></a>Slutlig distribution
Sista distributionen och specifika stegen, särskilt med avseende på distributionen av SAP utökad övervakning, finns i den [Deployment Guide][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Åtkomst till SAP-system som körs i virtuella Azure-datorer
Endast molnbaserad scenarier kanske du vill ansluta till dessa SAP-system i det offentliga internet med hjälp av Användargränssnittet för SAP. I dessa fall måste följande procedurer som ska användas.

Senare i dokumentet diskuteras andra större scenariot, ansluta till SAP-system i mellan lokala distributioner som har en plats-till-plats-anslutning (VPN-tunnel) eller Azure ExpressRoute-anslutning mellan lokala system och Azure system.

### <a name="remote-access-to-sap-systems"></a>Fjärråtkomst till SAP-system
Med Azure Resource Manager innehåller inga slutpunkter längre som den tidigare klassiska modellen. Alla portar för en Azure ARM-VM är öppna så länge som:

1. Ingen Nätverkssäkerhetsgrupp har definierats för undernätet eller nätverksgränssnittet. Nätverkstrafik till virtuella Azure-datorer kan vara skyddad via så kallade ”Nätverkssäkerhetsgrupper”. Mer information finns i [vad är en Nätverkssäkerhetsgrupp (NSG)?][virtual-networks-nsg]
2. Någon Azure belastningsutjämnare har definierats för nätverksgränssnittet   

Se arkitektur skillnaden mellan klassiska modellen och ARM enligt beskrivningen i [i den här artikeln][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Konfigurationen av SAP-systemet och SAP GUI-anslutning för endast molnbaserad scenario
Finns den här artikeln som beskriver information i det här avsnittet: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Ändra brandväggsinställningar inom VM
Det kan vara nödvändigt att konfigurera brandväggen på de virtuella datorerna för att tillåta inkommande trafik till SAP-system.

- - -
> ![Windows][Logo_Windows] Windows
>
> Windows-brandväggen i en distribuerad virtuell dator i Azure är aktiverat som standard. Nu måste du tillåta SAP-Port som ska öppnas, annars SAP-GUI kommer inte kunna ansluta.
> Gör så här:
>
> * Öppna kontrollpanelen\system och säkerhet\windows-brandväggen till att ”avancerade inställningar'.
> * Nu ska du högerklicka på regler för inkommande trafik och välj ”ny regel'.
> * I den följande guiden valt att skapa en ny regel 'Port'.
> * I nästa steg i guiden, lämnar du inställningen på TCP- och de portnummer som du vill öppna. Eftersom vår SAP instans-ID är 00, tog vi 3200. Om din instans har en annan instans., bör den port som du har definierat tidigare baserat på antalet instans öppnas.
> * Du måste behålla objektet ”Tillåt anslutningen' som kontrolleras i nästa steg i guiden.
> * Du måste definiera om regeln gäller för domän, privata och offentliga nätverk i nästa steg i guiden. Justera det vid behov efter dina behov. Dock behöver ansluter med SAP GUI utifrån via ett offentligt nätverk, du ha regeln ska gälla för ett offentligt nätverk.
> * I det sista steget i guiden måste du ge regeln ett namn och spara regeln genom att trycka på ”Slutför”
>
> Regeln träder i kraft omedelbart.
>
> ![Definitionen av port][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Linux-avbildningar i Azure Marketplace aktiverar inte iptables brandväggen som standard och anslutning till SAP-system ska fungera. Om du har aktiverat iptables eller en annan brandvägg finns i dokumentationen för iptables eller använda brandväggen för att tillåta inkommande tcp-trafik till port 32xx (där xx är system antalet SAP-system).
>
>

- - -
#### <a name="security-recommendations"></a>Säkerhetsrekommendationer
SAP-GUI ansluter inte direkt till en SAP-instans (port 32xx) som körs, men först ansluter via port öppna till SAP meddelandet serverprocessen (port 36xx). Tidigare var mycket samma port används av servern meddelande för intern kommunikation till instanser för programmet. För att förhindra lokalt programservrar oavsiktligt kommunicerar med en message-server i Azure intern kommunikationsportar kan ändras. Vi rekommenderar starkt att ändra intern kommunikation mellan SAP message-servern och dess programinstanser till ett annat portnummer på datorer som har varit klonad från lokalt system, t.ex en klon av utveckling för projektet tester osv. Detta kan göras med standardparametern profil:

> rdisp/msserv_internal
>
>

enligt beskrivningen i: <https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm>

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Begreppet endast molnbaserad distribution av SAP-instanser
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Enskild virtuell dator med SAP NetWeaver demo/utbildning scenario
![Kör enskild VM SAP Demo med samma VM, separat i Azure Cloud Services][planning-guide-figure-1700]

I det här scenariot (finns i kapitlet [endast molnbaserad] [ planning-guide-2.1] i det här dokumentet) vi implementera en typisk utbildning-demon system scenario där fullständig utbildning/demo-scenariot finns i en enda virtuell dator. Vi förutsätter att distributionen görs via avbildningen mallar för Virtuella datorer. Vi förutsätter också att flera av dessa demo/utbildningar virtuella datorer behöver distribueras med de virtuella datorerna som har samma namn.

Antas att du har skapat en VM-avbildning som beskrivs i vissa avsnitt i kapitel [förbereda virtuella datorer med SAP för Azure] [ planning-guide-5.2] i det här dokumentet.

En händelsesekvens att implementera scenariot ser ut så här:

[comment]: <> (MShermannd TODO behöver ge ARM prov / beskrivning använder json-mall + förtydligande om unikt namn på virtuell dator i ARM virtuellt nätverk)   
##### <a name="powershell"></a>PowerShell
* Skapa en ny grupp resoure för varje utbildning/demo liggande

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```

* Skapa ett nytt lagringskonto

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Skapa ett nytt virtuellt nätverk för varje utbildning/demo liggande om du vill aktivera användning av samma värdnamn och IP-adresser. Det virtuella nätverket skyddas av en Nätverkssäkerhetsgrupp som bara tillåter trafik till port 3389 att aktivera åtkomst via fjärrskrivbord och port 22 för SSH.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Skapa en ny offentlig IP-adress som kan användas för att få åtkomst till den virtuella datorn från internet

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Skapa ett nytt nätverksgränssnitt för den virtuella datorn

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Skapa en virtuell dator. Endast molnbaserad scenariot har varje virtuell dator samma namn. SAP-SID för SAP NetWeaver instanser i dessa virtuella datorer kommer att samma samt. Namnet på den virtuella datorn måste vara unikt i Azure-resursgrupp, men i olika Azure-resursgrupper kan du köra virtuella datorer med samma namn. Standardkonto för Windows eller 'rot' för Linux-administratörer' är inte giltiga. Ett nytt användarnamn för administratören måste definieras tillsammans med ett lösenord. Storleken på den virtuella datorn måste också definieras.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES" -Skus "12" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="os"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"
$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Du kan också lägga till ytterligare diskar och återställa nödvändigt innehåll. Tänk på att alla blob-namn (URL: er till blobarna) måste vara unikt i Azure.

```powershell
# Optional: Attach additional data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM
```

##### <a name="cli"></a>CLI
Följande exempelkod kan användas på Linux. För Windows, måste använda PowerShell enligt beskrivningen ovan eller anpassa exempel för att använda % rgName % i stället för $rgName och ange miljövariabeln med hjälp av kommandot Windows *ange*.

* Skapa en ny grupp resoure för varje utbildning/demo liggande

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
azure group create $rgName "North Europe"
```

* Skapa ett nytt lagringskonto

```
azure storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku-name LRS $rgNameLower
```

* Skapa ett nytt virtuellt nätverk för varje utbildning/demo liggande om du vill aktivera användning av samma värdnamn och IP-adresser. Det virtuella nätverket skyddas av en Nätverkssäkerhetsgrupp som bara tillåter trafik till port 3389 att aktivera åtkomst via fjärrskrivbord och port 22 för SSH.

```
azure network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

azure network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
azure network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group-name SAPERPDemoNSG
```

* Skapa en ny offentlig IP-adress som kan användas för att få åtkomst till den virtuella datorn från internet

```
azure network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --domain-name-label $rgNameLower --allocation-method Dynamic
```

* Skapa ett nytt nätverksgränssnitt för den virtuella datorn

```
azure network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-name SAPERPDemoPIP --subnet-name Subnet1 --subnet-vnet-name SAPERPDemoVNet
```

* Skapa en virtuell dator. Endast molnbaserad scenariot har varje virtuell dator samma namn. SAP-SID för SAP NetWeaver instanser i dessa virtuella datorer kommer att samma samt. Namnet på den virtuella datorn måste vara unikt i Azure-resursgrupp, men i olika Azure-resursgrupper kan du köra virtuella datorer med samma namn. Standardkonto för Windows eller 'rot' för Linux-administratörer' är inte giltiga. Ett nytt användarnamn för administratören måste definieras tillsammans med ett lösenord. Storleken på den virtuella datorn måste också definieras.

```
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn SUSE:SLES:12:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn RedHat:RHEL:7.2:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
#azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
```

* Du kan också lägga till ytterligare diskar och återställa nödvändigt innehåll. Tänk på att alla blob-namn (URL: er till blobarna) måste vara unikt i Azure.

```
# Optional: Attach additional data disks
azure vm disk attach-new --resource-group $rgName --vm-name SAPERPDemo --size-in-gb 1023 --vhd-name datadisk
```

##### <a name="template"></a>Mall
Du kan använda exempelmallarna i azure-snabbstartsmallar databasen på github.

* [Enkel virtuell Linux-dator](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Enkla Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Virtuell dator från avbildningen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Implementera en uppsättning virtuella datorer som behöver kommunicera i Azure
Det här scenariot med endast molnbaserad är ett typiskt scenario för utbildning och demo syfte var programvaran som representerar demo/utbildning scenariot sträcker sig över flera virtuella datorer. De olika komponenterna som är installerade på olika virtuella datorer som behöver kommunicera med varandra. Igen, i det här scenariot någon lokal nätverksanslutning eller scenarion med flera lokaler krävs.

Det här scenariot är en utökning av installationen som beskrivs i kapitlet [enskild virtuell dator med SAP NetWeaver demo/utbildning scenariot] [ planning-guide-7.1] i det här dokumentet. I det här fallet läggs flera virtuella datorer till en befintlig resursgrupp. I följande exempel består utbildning liggande av en SAP ASCS/SCS virtuell dator, en virtuell dator som kör en DBMS och en SAP Application Server-instans VM.

Innan du skapar det här scenariot måste du tänka på grundläggande inställningar som redan utförs i scenariot innan.

#### <a name="resource-group-and-virtual-machine-naming"></a>Resursgrupp och namngivning av virtuell dator
Alla resursgruppnamn måste vara unika. Utveckla egna namngivningsschemat för dina resurser, exempelvis `<rg-name`>-suffix.

Namnet på virtuella datorn måste vara unikt inom resursgruppen.

#### <a name="setup-network-for-communication-between-the-different-vms"></a>Konfigurera nätverket för kommunikation mellan olika virtuella datorer
![Uppsättning virtuella datorer i Azure-nätverk][planning-guide-figure-1900]

För att förhindra namnkonflikter med kloner av samma utbildning/demo landskap, måste du skapa ett Azure Virtual Network för varje liggande. DNS-namnmatchningen kommer att tillhandahållas av Azure eller konfigurera egna DNS-server utanför Azure (inte till ytterligare diskuteras här). I det här scenariot konfigurerar vi inte våra egna DNS. Kommunikation via värdnamn vara aktiverat för alla virtuella datorer i ett Azure Virtual Network.

Orsaker att separera utbildning eller demo landskap av virtuella nätverk och inte bara resursgrupper kan vara:

* SAP-liggande som ställs in måste egna AD/OpenLDAP och en domänserver måste vara en del av var och en av landskap.  
* SAP-liggande som ställs in har komponenter som behöver arbeta med fasta IP-adresser.

Mer information om virtuella Azure-nätverk och hur du definierar dem finns i [i den här artikeln][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Distribuera SAP virtuella datorer med företagets nätverksanslutning (mellan platser)
Du kör en SAP-liggande och vill dela upp distributionen mellan minimalt för avancerade DBMS-servrar, lokala virtualiserade miljöer för programmet lager och mindre 2-skikts konfigurerat SAP-system och Azure IaaS. Grundläggande antas att SAP system inom en SAP liggande behöver kommunicera med varandra och många andra programkomponenter som distribueras i företaget, oberoende av deras distribution form. Det bör också finnas några skillnader som introducerades av formuläret distributionen för användaren ansluta till SAP-Gränssnittet eller andra gränssnitt. Dessa villkor kan endast vara uppfyllda när vi har den lokala Active Directory/OpenLDAP och DNS-tjänster som utökats till Azure system via plats-till-plats/flera plats-anslutning eller privata anslutningar som Azure ExpressRoute.

För att få mer bakgrund på implementeringsinformation om SAP på Azure, bör du läsa kapitel [begrepp Cloud-Only distribution av SAP instanser] [ planning-guide-7] i det här dokumentet som beskriver vissa konstruktioner grunderna i Azure och hur de ska användas med SAP-program i Azure.

### <a name="scenario-of-a-sap-landscape"></a>Scenario för en SAP-liggande
Scenarion med flera lokaler kan beskrivas ungefär som i graphics nedan:

![Plats-till-plats-anslutning mellan lokala och Azure tillgångar][planning-guide-figure-2100]

Scenariot ovan beskriver ett scenario där lokalt AD/OpenLDAP och DNS har utökats till Azure. På den lokala sidan är ett vissa IP-adressintervall reserverat per Azure-prenumeration. IP-adressintervallet ska tilldelas ett Azure Virtual Network på Azure-sidan.

#### <a name="security-considerations"></a>Säkerhetsöverväganden
Minimikravet används för säker kommunikationsprotokoll, till exempel SSL/TLS för webbläsaråtkomst eller VPN-baserade anslutningar för systemåtkomst till Azure-tjänster. Antas företag att hantera VPN-anslutning mellan sina företagets nätverk och Azure väldigt annorlunda. Vissa företag kanske blankly öppna alla portar. Vissa andra företag kanske vill ha mycket exakt vilka portar som de behöver för att öppna, osv.

I tabellen nedan vanliga SAP visas kommunikationsportar. Det är i princip räcker för att öppna port för SAP-gateway.

| Tjänst | Portnamn | Exempel `<nn`> = 01 | Standardintervallet (min max) | Kommentar |
| --- | --- | --- | --- | --- |
| Dispatcher |sapdp`<nn>` finns * |3201 |3200 – 3299 |SAP Dispatcher, används av Java och SAP GUI för Windows |
| Meddelande-server |sapms`<sid`> finns ** |3600 |ledigt sapms`<anySID`> |SID = SAP-System-ID |
| Gateway |sapgw`<nn`> finns * |3301 |Ledigt |SAP-gateway som används för CPIC och RFC kommunikation |
| SAP-router |sapdp99 |3299 |Ledigt |Endast CI (central instans) Tjänstnamn kan omtilldelas i /etc/services till ett godtyckligt värde efter installationen. |

*) nn = SAP-instansnummer

*) sid = SAP-System-ID

Mer detaljerad information om portar som behövs för olika SAP produkter eller tjänster som SAP produkter hittar du här <http://scn.sap.com/docs/DOC-17124>.
Med det här dokumentet bör du kunna öppna dedikerade portarna i VPN-enheten krävs för specifika SAP-produkter och scenarier.

Andra säkerhetsmetoder när du distribuerar virtuella datorer i ett sådant scenario är att skapa en [Nätverkssäkerhetsgruppen] [ virtual-networks-nsg] att definiera regler för åtkomst.

### <a name="dealing-with-different-virtual-machine-series"></a>Med en annan virtuell dator-serie
Under de senaste 12 månaderna Microsoft lagt till många fler VM-typer som skiljer sig något i antal vCPUs, minne eller viktigare på maskinvara som den körs på. Inte alla dessa virtuella datorer stöds med SAP (se stöds VM typer i SAP-kommentar [1928533]). Vissa av dessa virtuella datorer körs på olika värden maskinvara generationer. Dessa värden maskinvara generationer distribueras om i Granulariteten för en Azure-Skalningsenhet. Sätt fall kan uppstå där olika VM-storlekar som du valde kan inte köras på samma Skalningsenheten. En Tillgänglighetsuppsättning har begränsad möjlighet att span skalenheter baserat på olika typer av maskinvara.  T.ex. Om du vill köra DBMS på A5 A11 VM- och programnivå SAP på G-serien virtuella datorer, skulle du tvungen att distribuera en enda SAP-system eller annan SAP-system inom olika Tillgänglighetsuppsättningar.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Skriva ut på en lokal nätverksskrivare från SAP-instans i Azure
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Utskrift över TCP/IP i scenarion med flera lokaler
Ställa in lokala TCP/IP baserat nätverksskrivare i en Azure VM är den totala desamma som företagets nätverk, förutsatt att du har en VPN plats-till-plats-tunneln eller ExpressRoute-anslutning upprättas.

- - -
> ![Windows][Logo_Windows] Windows
>
> Gör så här:
>
> * Vissa nätverksskrivare har en konfigurationsguide som gör det enkelt att konfigurera din skrivare i en Azure VM. Om inga guiden program har distribuerats med skrivaren är ”manuell” sätt att konfigurera skrivaren att skapa en ny TCP/IP-skrivarport.
> * Öppna Kontrollpanelen -> enheter och skrivare > Lägg till en skrivare
> * Välj Lägg till en skrivare med hjälp av en TCP/IP-adress eller värdnamn
> * Ange IP-adressen för skrivaren
> * Skrivarport standard 9100
> * Installera rätt skrivardrivrutin manuellt om det behövs.
>
> ![Linux][Logo_Linux] Linux
>
> * som standard procedur för att installera en skrivare i nätverket för Windows bara uppföljning
> * Följ bara offentliga Linux-guider för [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) eller [Red Hat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) på hur du lägger till en skrivare.
>
>

- - -
![Nätverket utskrift][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Värdbaserad skrivare över SMB (delad skrivare) i scenarion med flera lokaler
Värdbaserad skrivare är inte nätverks-kompatibla avsiktligt. Men en värdbaserad skrivare kan delas mellan datorer i ett nätverk som skrivaren är ansluten till en dator som ska slås på. Ansluta ditt företags nätverk plats-till-plats eller ExpressRoute och delar en lokal skrivare. SMB-protokollet använder NetBIOS i stället för DNS som namntjänst. NetBIOS-värdnamn kan skilja sig från DNS-värdnamn. I vanliga fall har NetBIOS-värdnamn och DNS-värdnamnet är identiska. DNS-domänen gör inte mening i området NetBIOS-namn. Det fullständigt kvalificerade DNS-värdnamn som består av DNS-värdnamn och DNS-domän måste därför inte användas i området NetBIOS-namn.

Skrivarresursen identifieras med ett unikt namn i nätverket:

* Värdnamnet för den SMB-Server (alltid behövs).
* Namnet på resursen (alltid behövs).
* Namnet på domänen är om delar skrivaren inte i samma domän som SAP-system.
* Ett användarnamn och ett lösenord kan dessutom krävas för att komma åt skrivarresursen.

Anvisningar:

- - -
> ![Windows][Logo_Windows] Windows
>
> Dela en lokal skrivare.
> Öppna Utforskaren och anger resursnamnet för skrivaren i Azure-VM.
> En skrivare installationsguiden hjälper dig igenom installationsprocessen.
>
> ![Linux][Logo_Linux] Linux
>
> Här följer några exempel på dokumentation om hur du konfigurerar nätverksskrivare i Linux eller inklusive kapitel rörande utskrift i Linux. Den fungerar på samma sätt i en Azure Linux VM så länge som den virtuella datorn är en del av en VPN-anslutning:
>
> * SLES <_Share_or_Windows_Share https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba)>
> * RHEL <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-printing-smb-printer.html>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB-skrivare (skrivare vidarebefordran)
Fjärrskrivbordstjänster förmåga att ge användare åtkomst till sina enheter för lokal skrivare i en fjärrsession är inte tillgängliga i Azure.

- - -
> ![Windows][Logo_Windows] Windows
>
> Mer information om utskrift i Windows finns här: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integrering av SAP Azure system till korrigering och transportsystem (Proportionerlig) mellan platser
SAP ändrings- och Transport System (Proportionerlig) måste konfigureras för att exportera och importera begäran över system i liggande. Vi förutsätter att development instanser av ett SAP-system (utveckling) finns i Azure quality assurance (QA) och produktiva system (PRD) är lokalt. Vi förutsätter dessutom att det finns en central transport-katalog.

##### <a name="configuring-the-transport-domain"></a>Konfigurera Transport-domän
Konfigurera Transport domänen på datorn som du angett som domänkontrollant Transport enligt beskrivningen i [konfigurerar domänkontrollanten Transport](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). En systemanvändare TMSADM kommer att skapas och nödvändiga RFC målet kommer att skapas. Du kan kontrollera anslutningarna RFC med transaktion SM59. Värdnamnsmatchning måste aktiveras i transport-domän.

Anvisningar:

* I vårt scenario valt lokala QAS system blir CTS-domänkontrollanten. Anropa transaktion STM. Dialogrutan Proportionerlig visas. En dialogruta för Konfigurera Transport domän visas. (Den här dialogrutan visas bara om du ännu inte har konfigurerat en transport-domän.)
* Kontrollera att den automatiskt skapade användaren TMSADM är auktoriserad (SM59 -> ABAP anslutning -> TMSADM@E61.DOMAIN_E61 -> information -> Utilities(M) auktorisering Test ->). Den första skärmen i transaktionen STM ska visa att systemet SAP nu fungerar som domänkontrollant i domänen transport som visas här:

![Första skärmen transaktion STM på domänkontrollanten][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Inklusive SAP-system i domänen Transport
Sekvens med inklusive en SAP-system i en domän för transport ser ut som följer:

* Gå till transportsystem (klient 000) och anropa transaktion STM på systemets DEV i Azure. Välj andra konfiguration från dialogrutan och fortsätt med inkluderar System i domänen. Ange den domänkontrollant som målvärden ([inklusive SAP-system i domänen Transport](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Systemet nu väntar på att tas med i domänen transport.
* Av säkerhetsskäl måste du sedan gå tillbaka till domänkontrollanten för att bekräfta din begäran. Välj Systemöversikt och Godkänn av systemet väntar. Bekräfta uppmaningen och konfigurationen som ska distribueras.

SAP systemet innehåller nu information om alla övriga SAP system i domänen transport. Systemets SAP-adressdata skickas till alla andra SAP-system på samma gång och SAP-systemet har angetts i profilen för transport av kontrollprogrammet för transporten. Kontrollera om RFC: er och åtkomst till Transportkatalogen i domänen att fungera.

Fortsätta med konfigurationen systemet transport som vanligt beskrivs i dokumentationen för [ändrings- och Transport System](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Anvisningar:

* Kontrollera att din STM lokalt är korrekt konfigurerad.
* Se till att värdnamnet för domänkontrollanten Transport kan lösas genom den virtuella datorn på Azure och vice visa.
* Anropet transaktion STM -> andra Configuration -> inkluderar System i domänen.
* Bekräfta anslutningen i på lokala Proportionerlig systemet.
* Konfigurera transport vägar, grupper och lager som vanligt.

I plats-till-plats anslutna mellan lokala scenarier, kan fördröjning mellan lokala och Azure fortfarande vara betydande. Om vi följer sekvensen för att transportera objekt genom utveckling och testsystem till produktion eller tänka på att tillämpa transporter eller stöd för paket till de olika system, inser du att, beroende på platsen för central transport katalog, några av systemen uppstår fördröjningar läsning eller skrivning av data i transportkatalogen centrala. Situationen liknar SAP liggande konfigurationer där olika system sprids via olika datacenter med stor avstånd mellan-datacenter.

För att undvika sådana svarstid och ha system fungerar Snabb läsning eller skrivning till eller från Transportkatalogen, kan du konfigurera två STM transport domäner (en för lokalt och en med system i Azure och länka transport-domäner. Kontrollera den här dokumentationen som beskriver principerna bakom detta begrepp i SAP Proportionerlig: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Anvisningar:

* Konfigurera en transport domän på varje plats (lokalt och Azure) med transaktion STM <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Länka domäner med en länk i domänen och bekräfta länken mellan de två domänerna.
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Distribuera konfigurationen av det länkade systemet.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC trafik mellan SAP-instanser som finns i Azure och lokala (mellan platser)
RFC trafik mellan datorer som finns lokalt och i Azure måste ska fungera. Att konfigurera en anslutning anropet transaktion SM59 i ett källsystem där du måste definiera ett RFC-anslutning till målsystemet. Konfigurationen liknar standardinställningen av en RFC-anslutning.

Vi förutsätter som i scenariot mellan platser, virtuella datorer som kör SAP-system som behöver kommunicera med varandra i samma domän. Därför installationen av en RFC-anslutning mellan SAP-system skiljer sig inte från konfigurationsstegen och indata i lokala scenarier.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Åtkomst till ”lokal' fileshares från SAP-instanser som finns i Azure eller tvärtom
SAP-instanser som finns i Azure som behöver åtkomst till filresurser som är i företagets lokaler. Dessutom behöver lokal SAP instanser åtkomst till filresurser som finns i Azure. Om du vill aktivera filresurser måste du konfigurera behörigheter och delningsalternativ på det lokala systemet. Se till att öppna portar i VPN eller ExpressRoute-anslutning mellan Azure och ditt datacenter.

## <a name="supportability"></a>Support
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure övervakningslösning för SAP
För att aktivera övervakning av uppdrag kritiska SAP-system på Azure SAP övervakningsverktyg SAPOSCOL eller SAP Värdagenten hämta data från Azure virtuella tjänstvärden via ett Azure-övervakning tillägg för SAP. Eftersom kraven av SAP var mycket specifik till SAP-program, valt Microsoft att inte Allmänt genomföra de nödvändiga funktionerna i Azure, men lämna det för kunder att distribuera de nödvändiga komponenter för övervakning och konfigurationer till sina virtuella datorer som körs i Azure. Dock automatiskt distribution och livscykeln för hantering av övervakning komponenter främst av Azure.

#### <a name="solution-design"></a>Lösningsdesign
Lösningen som utvecklats för att aktivera övervakning av SAP baseras på arkitekturen på Azure VM-agenten och ramverket. Uppfattning av framework Azure VM-agenten och tillägget är att tillåta installation av programvara program tillgängliga i galleriet Azure VM-tillägget på en virtuell dator. Principen syftet med detta begrepp är att (i fall Azure övervakning tillägget för SAP) distribution av särskilda funktioner i en virtuell dator och konfigurationen av sådan programvara vid tidpunkten för distribution.

Sedan februari 2014 injekteras 'Azure VM-agenten, som möjliggör hantering av specifika Azure VM-tillägg i den virtuella datorn i virtuella Windows-datorer som standard på Skapa en virtuell dator i Azure Portal. Agenten är redan en del av Azure Marketplace-avbildning vid SUSE eller Red Hat Linux den virtuella datorn. Om en skulle överför en Linux VM från lokal till Azure VM-agenten måste installeras manuellt.

De grundläggande byggstenarna för övervakning lösningen i Azure för SAP ser ut så här:

![Tillägg för Microsoft Azure-komponenter][planning-guide-figure-2400]

I block diagrammet ovan visas en del av övervakningslösning SAP finns i Azure VM-avbildning och Azure-tillägg-galleriet som är en globalt replikerad databas som hanteras av Azure-åtgärder. Ansvarar för gemensamma SAP/MS-teamet arbetar med Azure implementeringen av SAP ska fungera med Azure-åtgärder för att publicera nya versioner av Azure övervakning tillägget för SAP. Azure övervakning tillägget för SAP använder tillägget för Microsoft Azure Diagnostics (BOMULLSTUSS) eller Linux Azure Diagnostics (LAD) för att hämta nödvändig information.

När du distribuerar en ny Windows VM läggs 'Azure VM Agent' automatiskt till den virtuella datorn. Funktionen för den här agenten är att samordna inläsning och konfigurationen av Azure-tillägg för övervakning av SAP NetWeaver-system. För Linux virtuella datorer ingår på Azure VM-agenten redan i Azure Marketplace OS-avbildningen.

Det finns ett steg som måste utföras av kunden. Detta är aktivering och konfiguration av insamling av prestanda. Processen som är relaterade till konfigurationen automatiskt efter en PowerShell-skript eller CLI-kommando. PowerShell-skriptet kan hämtas i Microsoft Azure Script Center som beskrivs i den [Deployment Guide][deployment-guide].

Det ser ut så övergripande arkitektur i Azure övervakningslösning SAP:

![Azure övervakningslösning för SAP NetWeaver][planning-guide-figure-2500]

**Exakt anvisningar och detaljerade anvisningar för att använda dessa PowerShell-cmdlets eller CLI kommandot under distributioner, följ instruktionerna den [Deployment Guide][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integrering av Azure finns SAP-instans i SAProuter
SAP-instanser som körs i Azure måste vara tillgänglig från SAProuter samt.

![SAP-Router-nätverksanslutning][planning-guide-figure-2600]

En SAProuter aktiverar TCP/IP-kommunikation mellan deltagande system om det finns ingen direkt IP-anslutning. Detta ger fördelen att någon slutpunkt till slutpunkt-anslutning mellan partner för kommunikation krävs på nätverksnivån. SAProuter lyssnar på port 3299 som standard.
Om du vill ansluta SAP instanser via en SAProuter måste du ge SAProuter sträng och värden namn med alla försök att ansluta.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Hittills fokus i dokumentet har SAP NetWeaver i allmänhet eller SAP NetWeaver ABAP stacken. I det här små avsnittet listas särskilda överväganden för SAP Java-stacken. En av de viktigaste SAP NetWeaver Java uteslutande baserat program är SAP Enterprise Portal. Andra SAP NetWeaver baserat program som SAP PI och SAP lösning Manager använder både SAP NetWeaver ABAP och Java-stackar. Det har därför behöver Överväg specifika aspekter som är relaterade till den SAP NetWeaver Java-stacken.

### <a name="sap-enterprise-portal"></a>SAP Enterprise Portal
Installationen av en SAP-Portal i en virtuell Azure-dator skiljer sig inte från en på lokal installation om du distribuerar i mellan lokala scenarier. Eftersom DNS görs av lokal, kan du göra inställningar för enskilda instanser som konfigurerade lokalt. Rekommendationer och begränsningar som beskrivs i det här dokumentet hittills gäller i allmänhet för ett program som SAP Enterprise Portal eller SAP NetWeaver Java-stacken.

![Exponerade SAP-portalen][planning-guide-figure-2700]

En särskild distributionsscenariot som vissa kunder är direkt exponering av SAP Enterprise Portal till Internet när den virtuella värden är ansluten till företagets nätverk via plats-till-plats VPN-tunnel eller ExpressRoute. Du måste se till att vissa portar är öppna och inte blockeras av en brandvägg eller säkerhetsgrupp för sådana situationer. Samma säkerhetsnivån måste användas när du vill ansluta till en SAP Java-instans från lokalt i en situation med endast molnet.

Den inledande URI är http (s):`<Portalserver`>: 5XX00/irj där porten utgörs av 50000 plus (Systemnumber × 100). Standard portal URI SAP systemets 00 är `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. Mer information, ta en titt en <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Slutpunktskonfiguration][planning-guide-figure-2800]

Kontrollera den här dokumentationen om du vill anpassa URL och/eller portar för SAP Enterprise Portal:

* [Ändra Portal-URL](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Ändra standardportnumren Portal-portnummer](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

<a name="7cf991a1-badd-40a9-944e-7baae842a058"></a>
## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Hög tillgänglighet och Disaster Recovery (DR) för SAP NetWeaver körs på Azure Virtual Machines
### <a name="definition-of-terminologies"></a>Definition av termer som gäller
Termen **hög tillgänglighet (HA)** beror vanligtvis på en uppsättning tekniker som minimerar IT avbrott genom att tillhandahålla affärskontinuitet av IT-tjänster via redundanta, feltolerant eller failover skyddade komponenter i den **samma** datacenter. I vårt fall, inom en Azure-Region.

**Katastrofåterställning (DR)** är också på minimera avbrott i IT-tjänster och deras återställning men tvärs över **olika** datacenter som vanligtvis finns hundratals kilometer direkt. I vårt fall vanligtvis mellan olika Azure-regioner inom samma geopolitiska region eller i enlighet med du som kund.

### <a name="overview-of-high-availability"></a>Översikt över hög tillgänglighet
Vi kan avgränsa diskussion om SAP hög tillgänglighet i Azure i två delar:

* **Hög tillgänglighet för Azure-infrastrukturen**, t.ex. HA beräkning (VM), nätverk, lagring etc. och dess fördelar för att öka tillgängligheten för SAP-program.
* **Hög tillgänglighet för SAP-program**, t.ex. HA av SAP programvarukomponenter:
  * SAP-programservrar
  * SAP ASCS/SCS-instans
  * DB-server

och hur den kan kombineras med Azure-infrastrukturen hög tillgänglighet.

SAP hög tillgänglighet i Azure har vissa skillnader i jämförelse med hög tillgänglighet för SAP i en lokal fysisk eller virtuell miljö. Följande dokumentet från SAP beskriver standardkonfigurationer SAP hög tillgänglighet i virtualiserade miljöer i Windows: <http://scn.sap.com/docs/DOC-44415>. Det finns ingen sapinst-integrerade SAP-HA konfiguration för Linux som om det finns för Windows. Om SAP HA lokalt för Linux hitta mer information här: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure-infrastrukturen med hög tillgänglighet
Det finns ingen enskild VM SLA på Azure Virtual Machines just nu. Att få en uppfattning om hur tillgängligheten för en enda virtuell dator kan se ut som om du bara kan skapa produkten av de olika tillgängliga Azure SLA: <https://azure.microsoft.com/support/legal/sla/>.

Basen för beräkningen är 30 dagar i månaden, eller 43200 minuter. Därför motsvarar 0,05% avbrottstid 21,6 minuter. Tillgängligheten för olika tjänster kommer som vanligt, multiplicera på följande sätt:

(Tillgänglighet Service #1/100) * (tillgänglighet Service #2/100) * (tillgänglighet Service #3/100) *...

Exempel:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 eller 99.75% övergripande tillgänglighet.

#### <a name="virtual-machine-vm-high-availability"></a>Virtuell dator (VM) med hög tillgänglighet
Det finns två typer av händelser i Azure-plattformen som kan påverka tillgängligheten för virtuella datorer: planerat underhåll och oplanerat underhåll.

* Planerat underhållshändelser är periodiska uppdateringar som har tillverkats av Microsoft för underliggande Azure-plattformen för att förbättra övergripande tillförlitlighet, prestanda och säkerhet för plattformsinfrastrukturen som de virtuella datorerna körs på.
* Oplanerat underhållshändelser inträffar när maskinvaru- eller fysisk infrastruktur underliggande din virtuella dator fel har uppstått på något sätt. Det kan vara lokala nätverksfel, lokala diskfel eller andra fel på racknivå. När sådant fel identifieras Azure-plattformen automatiskt att migrera den virtuella datorn från ohälsosamt fysisk server som värd för den virtuella datorn till en felfri fysisk server. Den här typen av händelser kan också göra att den virtuella datorn startas om, men det är ovanligt.

Mer information finns i den här dokumentationen: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure Storage-redundans
Data i Microsoft Azure Storage-konto replikeras alltid för att säkerställa hållbarhet och hög tillgänglighet, uppfyller Azure-serviceavtalet även i händelse av tillfälliga maskinvarufel för lagring

Eftersom Azure Storage är att ha 3 avbildningar av data som standard, RAID5 eller RAID1 över flera Azure-diskar är inte nödvändigt.

Mer information finns i den här artikeln: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Använda Azure-infrastrukturen VM omstart för att uppnå ”högre tillgänglighet” för SAP-program
Om du väljer att inte använda funktioner som Windows Server Failover Clustering WSFC- eller Linux motsvarande (det senare en inte stöds ännu i Azure i kombination med SAP-program), starta om Azure VM används för att skydda en SAP-systemet mot planerade och oplanerat stillestånd Azure fysiska serverinfrastruktur och övergripande underliggande Azure-plattformen.

> [!NOTE]
> Det är viktigt att nämna att starta om Azure VM främst skyddar virtuella datorer och inte program. Starta om VM inte ger hög tillgänglighet för SAP-program, men den erbjuder en viss nivå av infrastrukturens tillgänglighet och därför indirekt ”högre tillgänglighet” för SAP-system. Det finns också några SLA för den tid det tar att starta om en virtuell dator efter en planerad eller oplanerad värden avbrott. Den här metoden för 'med hög tillgänglighet är därför inte lämpar sig för viktiga komponenter av en SAP-system som (A) SCS eller DBMS.
>
>

Ett annat viktigt infrastruktur-element för hög tillgänglighet är lagring. T.ex. SERVICENIVÅAVTAL för Azure Storage är 99,9% tillgänglighet. Om en distribuerar du alla virtuella datorer med dess diskar till en enda Azure Storage-konto, potentiella Azure Storage otillgänglighet kommer att placeras i den Azure Storage-konto på alla virtuella datorer och även alla SAP komponenter som körs i dessa virtuella datorer.  

I stället för att alla virtuella datorer till en enda Azure Storage-konto, du kan också använda dedikerade konton för varje virtuell dator och på så sätt öka övergripande VM och SAP tillgänglighet genom att använda flera oberoende Azure Storage-konton.

En exempel-arkitekturen för en SAP NetWeaver system som använder Azure-infrastrukturen hög tillgänglighet kan se ut så här:

![Använda Azure-infrastrukturen hög tillgänglighet att uppnå SAP ”senare” tillgänglighet][planning-guide-figure-2900]

För kritiska SAP-komponenter uppnås vi följande hittills:

* Hög tillgänglighet för SAP-programservrar (AS)

SAP-programserverinstanser är redundanta komponenter. Varje SAP som instansen har distribuerats på egen virtuell dator som körs i en annan Azure-fel och uppgradera domän (se kapitlen [Feldomäner] [ planning-guide-3.2.1] och [uppgradera domäner][planning-guide-3.2.2]). Detta säkerställs med hjälp av Azure-Tillgänglighetsuppsättningar (finns i kapitlet [Azure-Tillgänglighetsuppsättningar][planning-guide-3.2.3]). Potentiella planerad eller oplanerad otillgänglighet en Azure-fel eller uppgradera domän kommer inte finns ett begränsat antal virtuella datorer med deras SAP instanser.
Varje SAP eftersom instansen är placerad i sin egen Azure Storage-konto – potentiella otillgänglighet ett Azure Storage-konto kommer att endast en virtuell dator med dess SAP instans. Men tänk på att det finns en gräns på Azure Storage-konton inom en Azure-prenumeration. Automatisk start av (A) SCS-instans efter omstart VM, kontrollera att ställa in Autostart-parametern i (A) SCS instans start-profil som beskrivs i kapitlet [med hjälp av Autostart för SAP instanser][planning-guide-11.5].
Läs även kapitel [hög tillgänglighet för SAP-programservrar] [ planning-guide-11.4.1] för mer information.

* *Högre* tillgänglighet för SAP (A) SCS-instans

Vi använder här Azure VM startas om för att skydda den virtuella datorn med installerad SAP (A) SCS-instans. Vid planerad eller oplanerad driftstopp på Azure-servrarna, virtuella datorer kommer att startas om på en annan server. Som tidigare nämnts kan skyddar starta om Azure VM främst virtuella datorer och inte program i det här fallet (A) SCS-instans. Via den virtuella datorn startar om ska vi nå indirekt ”högre tillgänglighet” för SAP (A) SCS-instansen. Se till att ange Autostart-parametern i (A) SCS instans start-profil som beskrivs i kapitlet om du vill kontrollera automatisk start av (A) SCS-instans efter omstart VM [med hjälp av Autostart för SAP instanser][planning-guide-11.5]. Det här betyder att (A)-SCS serverinstansen som en enskilda felpunkter (SPOF) körs i en enda virtuell dator är bestämmande faktor tillgängligheten för hela SAP-liggande.

* *Högre* tillgängligheten för DBMS-Server

Här, liknande till SCS SAP (A)-instans användningsfall, vi använda Azure VM startas om för att skydda den virtuella datorn med installerad programvara för DBMS och vi uppnår ”högre tillgänglighet” av DBMS programvara via virtuella datorn startar om.
DBMS som körs i en enda virtuell dator är också en SPOF och det är bestämmande faktor för tillgängligheten för hela SAP-liggande.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP hög tillgänglighet på Azure IaaS
Vi behöver skydda alla kritiska SAP komponenter, t.ex. redundant SAP programservrar, för att uppnå full SAP system hög tillgänglighet och unika komponenter (t.ex. enskilda felpunkter) som SAP (A) SCS-instans och DBMS.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Hög tillgänglighet för SAP-programservrar
För dialogrutan och servrar för SAP programinstanser behöver du inte tror att om en lösning för specifika hög tillgänglighet. Hög tillgänglighet bara uppnås genom redundans och därmed ha tillräckligt många dem i olika virtuella datorer. De ska alla placeras i samma Azure Tillgänglighetsuppsättningen att undvika att de virtuella datorerna kan uppdateras samtidigt under planerat underhåll driftstopp. Grundläggande funktioner som bygger på olika uppgradering och Feldomäner i en Azure-Skalningsenhet introducerades redan i kapitel [uppgradera domäner][planning-guide-3.2.2]. Azure Tillgänglighetsuppsättningar presenteras i kapitlet [Azure-Tillgänglighetsuppsättningar] [ planning-guide-3.2.3] i det här dokumentet.

Det finns inget obegränsat antal fel och uppgradera domäner som kan användas av en Azure Tillgänglighetsuppsättning inom en Skalningsenhet för Azure. Detta innebär att försätta ett antal virtuella datorer i en Tillgänglighetsuppsättning förr eller senare i det faktum att mer än en virtuell dator avslutas med samma fel eller uppgradera domän

Distribuerar några SAP programserverinstanser i sina dedikerade virtuella datorer och förutsatt att vi 5 Uppgradera domäner, på följande bild visar sig i slutet. Det faktiska max antalet fel och uppdatera domäner inom en tillgänglighetsuppsättning kan ändras i framtiden:

![Hög tillgänglighet för SAP programservrar i Azure][planning-guide-figure-3000]

Mer information finns i den här dokumentationen: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>Hög tillgänglighet för SAP (A) SCS-instans på Windows
Failover Cluster WSFC (Windows Server) är en lösning som används ofta för att skydda SAP (A) SCS-instans. Det är även integrerad i sapinst i form av en ”hög tillgänglighet-installation”. Vid denna tidpunkt kan Azure-infrastrukturen inte tillhandahålla funktioner för att ställa in nödvändiga Windows Server-redundanskluster på samma sätt som det har gjort lokalt.

Januari 2016 tillhandahåller Azure molnplattform som kör Windows operativsystem inte möjligheten att använda en delad klustervolym på en disk som delas mellan två virtuella Azure-datorer.

En giltig lösning är dock användningen av 3 tillverkare som ger en delad volym av synkrona och transparent diskreplikering som kan integreras i WSFC. Den här metoden innebär att den aktiva klusternoden har tillgång till en diskkopior vid en viss tidpunkt. Konfigurationen stöds för att skydda SAP (A) SCS-instans på Windows gästoperativsystemet på Azure Virtual Machines i kombination med 3 tillverkare SIOS DataKeeper januari 2016 denna hög tillgänglighet.

SIOS DataKeeper lösningen innehåller en delad disk klusterresurs för Windows-redundanskluster genom att:

* En ytterligare Azure-VHD som är kopplad till var och en av de virtuella datorerna (VM) som finns i en konfiguration för Windows-kluster
* SIOS DataKeeper Cluster Edition körs på båda noderna för VM
* Har konfigurerats på ett sätt som synkront speglar innehållet ytterligare VHD SIOS DataKeeper Cluster Edition ansluten bifogade volym från virtuella källdatorer till ytterligare virtuell Hårddisk volymen av virtuell dator.
* SIOS DataKeeper abstrahera käll- och lokala volymer och presentera dem för redundanskluster i Windows som en enda delad disk.

Du kan hitta all information om hur du installerar ett Windows-redundanskluster med SIOS Datakeeper och SAP i den [Clustering SAP ASCS-instans med redundanskluster för Windows Server på Azure med SIOS DataKeeper] [ ha-guide-classic] vitboken.

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Hög tillgänglighet för SAP (A) SCS-instansen på Linux
Från och med Dec 2015 finns även ingen motsvarighet till delad disk WSFC för Linux virtuella datorer i Azure. Alternativa lösningar med hjälp av 3 tillverkare som SIOS för Windows verifieras inte ännu för att köra SAP på Linux på Azure.

#### <a name="high-availability-for-the-sap-database-instance"></a>Hög tillgänglighet för SAP-databasinstans
SAP DBMS HA brukar baseras på två DBMS virtuella datorer där DBMS hög tillgänglighet funktionen används för att replikera data från den aktiva DBMS-instansen till en andra virtuell dator till en passiv DBMS-instans.

Hög tillgänglighet och Disaster recovery-funktioner för DBMS i allmänhet samt specifika DBMS beskrivs i den [DBMS Deployment Guide][dbms-guide].

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Hög tillgänglighet från slutpunkt till slutpunkt för fullständig SAP-System
Här är två exempel på en fullständig SAP NetWeaver HA arkitektur i Azure - en för Windows och en för Linux.
Begreppen som förklaras nedan kan behöva äventyras lite när du distribuerar många SAP-system och antalet virtuella datorer som distribueras överskrider den maximala gränsen på Lagringskonton per prenumeration. I sådana fall måste virtuella hårddiskar för virtuella datorer kombineras i ett Lagringskonto. Vanligtvis vill du göra det genom att kombinera virtuella hårddiskar för SAP programnivå virtuella datorer i olika SAP-system.  Vi också kombinera olika virtuella hårddiskar på en annan DBMS virtuella datorer i olika SAP-system i en Azure Storage-konto. Därmed Tänk på hur IOPS-gränser för Azure Storage-konton ( <https://azure.microsoft.com/documentation/articles/storage-scalability-targets> )

##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] Hög tillgänglighet i Windows
![SAP NetWeaver programarkitektur hög tillgänglighet med SQLServer i Azure IaaS][planning-guide-figure-3200]

Följande Azure konstruktioner används för SAP NetWeaver-systemet för att minimera effekten av infrastrukturen problem och korrigering:

* Hela systemet distribueras på Azure (obligatoriskt – DBMS layer, (A) SCS-instans och lager för hela programmet måste du köra på samma plats).
* Hela systemet körs inom en Azure-prenumeration (krävs).
* Hela systemet körs inom ett virtuellt Azure-nätverk (krävs).
* Uppdelning av de virtuella datorerna i en SAP-systemet till tre Tillgänglighetsuppsättningar är möjligt med alla de virtuella datorerna som hör till samma virtuella nätverk.
* Alla virtuella datorer som kör DBMS instanser av en SAP-systemet är i en Tillgänglighetsuppsättning. Vi förutsätter att det finns fler än en virtuell dator som kör DBMS instanser per system eftersom interna DBMS-hög tillgänglighet som används för funktioner, t.ex. SQL Server AlwaysOn eller Oracle Data Guard.
* Alla virtuella datorer som kör DBMS instanser använda sina egna storage-konto. DBMS data och loggfilen filer replikeras till en annan storage-konto med hjälp av DBMS hög tillgänglighet funktioner som synkroniserar data från ett lagringskonto. Inte finns en storage-konto kommer att en SQL-Windows-klusternod, men inte hela SQL Server-tjänsten.
* Alla virtuella datorer som kör (A) SCS-instans av en SAP-systemet är i en Tillgänglighetsuppsättning. I dessa virtuella datorer är att konfigurera Windows Sever klustret WSFC (Failover) att skydda (A) SCS-instans.
* Alla virtuella datorer (A) SCS-instanser körs använder sina egna lagringskonto. (A) SCS instansfiler och SAP globala mappen replikeras till ett annat lagringskonto med SIOS DataKeeper replikering från ett lagringskonto. Otillgänglighet ett lagringskonto gör att en nod i klustret (A) SCS Windows, men inte hela (A) SCS-tjänsten.
* ALLA de virtuella datorerna som representerar SAP server Applikationsnivån är i ett tredje Tillgänglighetsuppsättning.
* ALLA de virtuella datorerna kör SAP-programservrar använder sina egna lagringskonto. Inte finns en storage-konto kommer att en server för SAP-program, där andra SAP som fortsätter att köras.

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] Hög tillgänglighet på Linux
Arkitekturen för SAP hög tillgänglighet på Linux i Azure är ungefär desamma som för Windows som beskrivs ovan. Jan 2016 finns två begränsningar om:

* endast SAP ASE 16 stöds för närvarande på Linux på Azure utan några ASE replikeringsfunktioner.
* Det finns ingen SAP (A) SCS HA lösning stöds ännu på Linux på Azure

Följaktligen januari 2016 ett Azure-SAP-Linux-system kan inte få samma tillgänglighet som ett SAP Windows Azure-system på grund av saknas hög tillgänglighet för SCS (A)-instans och single instance SAP ASE databas.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Med hjälp av Autostart för SAP-instanser
SAP erbjuds funktioner för att starta SAP instanser direkt efter start av Operativsystemet i den virtuella datorn. Vilka de specifika stegen finns dokumenterade i SAP Knowledge Base-artikel [1909114] – hur instanser automatiskt med hjälp av parametern Autostart att starta SAP. Dock SAP inte rekommenderar använder inställningen längre eftersom det finns ingen kontroll i den ordning som instansen startas om, under förutsättning att mer än en virtuell dator har fått påverkas eller flera instanser körde per virtuell dator. Under förutsättning att ett typiskt Azure scenario för en SAP application server-instans i en virtuell dator och fall då en enda virtuell dator så småningom få startas, Autostart är inte mycket viktig och kan aktiveras genom att lägga till den här parametern:

    Autostart = 1

I profilen för start av SAP ABAP och/eller Java-instansen.

> [!NOTE]
> Parametern Autostart kan ha vissa downfalls samt. I större detalj utlöser parametern början av en SAP ABAP eller Java när Windows-/ Linux-tjänsterna på instansen har startats. Så är fallet har när operativsystem startar. Dock omstarter av SAP-tjänster finns också en gemensam sak för SAP programvara livscykelhantering funktioner som SUMMAN eller andra uppdateringar eller uppgraderingar. Dessa funktioner inte förväntar dig en instans som ska startas om automatiskt alls. Parametern Autostart ska vara inaktiverat innan du kör sådana uppgifter. Parametern Autostart bör också inte användas för SAP-instanserna är klustrade som CI-ASCS/SCS.
>
>

Se ytterligare information om autostart för SAP instanser här:

* [Starta/Stoppa SAP tillsammans med din Unix Server Starta/Stoppa](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Starta och stoppa SAP NetWeaver Hanteringsagenter](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Så här aktiverar du automatisk Start av HANA-databas](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Större 3-skikts SAP-system
Hög tillgänglighet aspekter av 3-skikts SAP konfigurationer fick beskrivs tidigare redan. Men vad händer om system där DBMS-serverkrav är för stort för att det finns i Azure, men programnivå SAP kan distribueras till Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>Platsen för 3-skikts SAP-konfigurationer
Det går inte för att dela program nivån sig själv eller i program- och DBMS tjänstnivån mellan lokala och Azure. En SAP-systemet är antingen helt distribuerade lokalt eller i Azure. Det är också inte stöd för några av de programservrar som kör lokalt och vissa andra i Azure. Det är startpunkten för diskussionen. Vi också stöder inte för att ha DBMS-komponenterna i en SAP-system och SAP server Applikationsnivån distribueras i två olika Azure-regioner. T.ex. DBMS i USA, västra och SAP programnivå i centrala USA. Orsaken till inte har stöd för sådana konfigurationer är latens känslighet för SAP NetWeaver-arkitekturen.

Dock under förra året data center partner utvecklat samtidigt platser till Azure-regioner. Dessa medarbetare platser är ofta mycket nära till fysiska Azure data datacenter i en Azure-Region. Korta avstånd och anslutning av tillgångar i samplacering via ExpressRoute till Azure kan resultera i en latens som är mindre än 2 MS. I sådana fall för att hitta DBMS-lagret (inklusive lagring SAN/NAS) i en samplacering och SAP är programnivå i Azure möjligt. Från och med Dec 2015 har vi inte alla distributioner som de som. Men olika kunder med icke-SAP application-distributioner använder redan dessa metoder.

### <a name="offline-backup-of-sap-systems"></a>Offline säkerhetskopiering av SAP-system
Beroende på hur SAP valt (2 eller 3-nivåer) det kan vara nödvändigt att säkerhetskopiering. Innehållet i Virtuellt datorn plus ha en säkerhetskopia av databasen. DBMS relaterade säkerhetskopieringar förväntas göras med metoderna i databasen. En detaljerad beskrivning av olika databaser finns i [DBMS guiden][dbms-guide]. Å andra sidan kan SAP-data säkerhetskopieras på ett sätt som offline (inklusive vad databasen) enligt beskrivningen i det här avsnittet online eller som beskrivs i nästa avsnitt.

Offlinesäkerhetskopiering kräver i princip en avstängning av den virtuella datorn via Azure Portal och en kopia av basdisk för VM samt alla anslutna virtuella hårddiskar till den virtuella datorn. Detta skulle bevara en punkt i tiden avbildningen av den virtuella datorn och dess associerade disk. Det rekommenderas att kopiera säkerhetskopiorna till en annan Azure Storage-konto. Därför förfarandet som beskrivs i kapitlet [kopierar diskar mellan Azure Storage-konton] [ planning-guide-5.4.2] i det här dokumentet skulle tillämpas.
Förutom avstängningen med en Azure-portalen kan också göra det via Powershell eller CLI som beskrivs här: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

En återställning av det aktuella tillståndet skulle består av borttagning av grundläggande VM samt grundläggande VM ursprungliga diskar och monterade virtuella hårddiskar, kopierar de sparade virtuella hårddiskarna till den ursprungliga Storage-konto och omdistribuera systemet.
Den här artikeln visar ett exempel så att den här processen i Powershell-skript: <http://www.westerndevs.com/azure-snapshots/>

Kontrollera att installera en ny SAP-licens eftersom restoing en säkerhetskopiering som beskrivs ovan skapar en ny maskinvarunyckel.

### <a name="online-backup-of-an-sap-system"></a>Onlinesäkerhetskopiering för en SAP-system
Säkerhetskopiering av DBMS utförs med DBMS-specifika metoder som beskrivs i den [DBMS guiden][dbms-guide].

Andra virtuella datorer i SAP-system kan säkerhetskopieras med funktioner för säkerhetskopiering av Azure virtuella datorer. Azure Virtual Machine säkerhetskopiering fick introduceras tidigt i 2015 och under tiden är en metod som standard för att säkerhetskopiera en fullständig virtuell dator i Azure. Azure-säkerhetskopiering lagrar säkerhetskopieringar i Azure och gör en återställning av en virtuell dator igen.

> [!NOTE]
> Från och med Dec 2015 med säkerhetskopiering behålls inte unikt ID för virtuell dator som används för SAP-licensiering. Det innebär att en återställning från en säkerhetskopiering måste du installera en ny SAP licensnyckel som den återställda virtuella datorn ska anses vara en ny virtuell dator och inte en ersättning för det tidigare som har sparats.
> Jan 2016 stöder Azure VM Backup inte virtuella datorer som distribueras med Azure Resourc Manager ännu.
>
> ![Windows][Logo_Windows] Windows
>
> Teoretiskt virtuella datorer som kör databaser kan säkerhetskopieras på ett konsekvent sätt om DBMS-system har stöd för Windows-VSS (Volume Shadow Copy Service <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx> ) som t.ex. SQL Server.
> Tänk dock som baseras på Virtuella Azure-säkerhetskopieringar i tidpunkt återställer databaser inte är möjligt. Därför vi rekommenderar att du genomför säkerhetskopieringar av databaser med DBMS-funktionalitet i stället för att förlita dig på Azure VM Backup
>
> För att bekanta dig med säkerhetskopiering av Azure virtuella datorer starta här: <https://azure.microsoft.com/documentation/articles/backup-azure-vms/>.
>
> Andra möjligheterna är att använda en kombination av Microsoft Data Protection Manager installeras i en virtuell dator i Azure och Azure Backup till databaser för säkerhetskopiering/återställning. Mer information hittar du här: <https://azure.microsoft.com/documentation/articles/backup-azure-dpm-introduction/>.  
>
> ![Linux][Logo_Linux] Linux
>
> Det finns ingen motsvarighet till Windows VSS i Linux. Endast filkonsekventa säkerhetskopior är därför möjligt men inte programkonsekvent säkerhetskopiering. SAP DBMS säkerhetskopieringen ska göras med hjälp av DBMS-funktioner. Filsystem som innehåller SAP relaterade data kan sparas, t.ex. använder tar som beskrivs här: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure som DR-plats för produktion SAP landskap
Eftersom Mid 2014 aktivera tillägg för olika komponenter runt Hyper-V, System Center och Azure användningen av Azure som DR-plats för virtuella datorer som körs lokalt baserat på Hyper-V.

En blogg med information om hur du distribuerar den här lösningen dokumenteras här: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>

## <a name="summary"></a>Sammanfattning
Viktiga punkter med hög tillgänglighet för SAP-system i Azure är:

* Vid denna tidpunkt, kan inte den SAP felpunkten skyddas på samma sätt som kan utföras i lokala distributioner. Det beror på den delade disken inte kan ännu genereras i Azure kluster utan att använda 3 tillverkare.
* Du måste använda DBMS-funktioner som inte är beroende av delad disk klustret teknik för DBMS-lagret. Information som finns dokumenterade i den [DBMS guiden][dbms-guide].
* Du bör använda Azure-Tillgänglighetsuppsättningar för att minimera effekten av problem inom Feldomäner i Azure-infrastruktur eller värden Underhåll:
  * Du bör ha en Tillgänglighetsuppsättning för SAP programnivå.
  * Du bör ha en separat Tillgänglighetsuppsättning för SAP DBMS-lagret.
  * Du bör inte använda samma Tillgänglighetsuppsättning för virtuella datorer i olika SAP-system.
* För säkerhetskopiering av SAP DBMS-lagret är Kontrollera den [DBMS guiden][dbms-guide].
* Säkerhetskopiera SAP dialogrutan instanser är lite meningsfullt eftersom det går snabbare att distribuera enkel dialogruta instanser.
* Säkerhetskopiera den virtuella datorn som innehåller den globala katalogen för SAP-systemet och med alla profiler för olika instanser särskilt meningsfullt och bör utföras med Windows Säkerhetskopiering eller t.ex. tar på Linux. Eftersom det är skillnad mellan Windows Server 2008 (R2) och Windows Server 2012 (R2), vilket gör det enklare att säkerhetskopiera med hjälp av den nyare Windows-Server släpper rekommenderar vi för att köra Windows Server 2012 (R2) som gästoperativsystem för Windows.
