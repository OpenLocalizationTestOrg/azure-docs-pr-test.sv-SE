---
title: "aaaAzure virtuella datorer planering och implementering för SAP NetWeaver | Microsoft Docs"
description: "Azure virtuella datorer planering och implementering för SAP NetWeaver"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d1e9fa13d847e4d8abb3c34fd352d1f4ece3717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Azure virtuella datorer planering och implementering för SAP NetWeaver
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
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
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
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

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

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
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
[sap-pam]:https://support.sap.com/pam
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
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-windows-capture-image-resource-manager]:../../windows/capture-image.md
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-create-upload-vhd-oracle]:../../linux/oracle-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
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
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
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
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure kan företag tooacquire beräkning och lagring resurser i minimal tid utan långa inköp cykler. Virtuella Azure-datorer utöka sina tillförlitlighet och tillgänglighet utan att behöva ytterligare resurser tillgängliga lokalt kan företag toodeploy klassisk program, som SAP NetWeaver baserat program i Azure. Azure Virtual Machine-tjänster stöder också korsanslutningar, vilket möjliggör företag tooactively integrera Azure virtuella datorer i sina lokala domäner sina privata moln och deras liggande för SAP-System.
I det här dokumentet beskriver hello grunderna i Microsoft Azure-dator och ger en beskrivning av överväganden för planering och implementering för SAP NetWeaver installationer i Azure och som sådana ska vara hello dokumentet tooread innan du startar faktiska distributioner av SAP NetWeaver på Azure.
hello papper kompletterar hello SAP dokumentationen och SAP anteckningar, som representerar hello primära resurser för installation och distribution av program på anges plattformar.

## <a name="summary"></a>Sammanfattning
Molntjänster är en term som används mycket som har blivit mer och mer betydelse inom hello IT-branschen, från små företag upp toolarge och multinationella företag.

Microsoft Azure är hello Molnplattform tjänster från Microsoft, vilket ger ett brett spektrum av nya möjligheter. Nu är kunder kan toorapidly etablera och avinstallation etablera program som en tjänst i molnet hello, så att de inte är begränsad tootechnical eller budgetering begränsningar. I stället för investera tid och budget i infrastrukturen för maskinvara, kan företag fokuserar på hello program, processer och dess fördelar för kunder och användare.

Med Microsoft Azure Virtual Machine-tjänsterna, erbjuder Microsoft en omfattande plattform för infrastruktur som en tjänst (IaaS). SAP NetWeaver-baserade program stöds på virtuella Azure-datorer (IaaS). Den här rapporten beskrivs hur tooplan och implementera SAP NetWeaver baserat program i Microsoft Azure som hello plattform för serveroperativsystem.

hello papper själva fokuserar på två huvudsakliga aspekter:

* hello första delen beskrivs två mönster för distribution som stöds för SAP NetWeaver baserat program på Azure. Här beskrivs också Allmänt hantering av Azure med SAP-distribution i åtanke.
* hello andra en del information implementera hello två olika scenarier beskrivs i hello första delen.

För ytterligare resurser finns i kapitlet [resurser] [ planning-guide-1.2] i det här dokumentet.

### <a name="definitions-upfront"></a>Definitioner förskott
I hello dokumentet använder vi hello följande villkor:

* IaaS: Infrastruktur som en tjänst
* PaaS: Plattform som en tjänst
* SaaS: Programvara som en tjänst
* ARM: Azure Resource Manager
* SAP komponent: ett enskilt SAP program, till exempel ECC BW, lösning Manager eller EP.  SAP-komponenter kan baseras på traditionell ABAP eller Java-teknik eller icke-NetWeaver baserat program, till exempel Business-objekt.
* SAP-miljö: en eller flera komponenter för SAP logiskt grupperade tooperform funktioner, till exempel utveckling, QAS, träning, DR eller produktion.
* SAP liggande: Refererar detta toohello hela SAP tillgångar i kundens IT-miljön. hello SAP liggande innehåller alla produktions- och icke-produktionsmiljöer.
* SAP System: hello kombination av DBMS lager- och programnivå av, till exempel en SAP ERP-utvecklingssystemet, SAP BW testsystemet, SAP CRM produktionssystem osv... I Azure-distributioner är det inte stöds toodivide dessa två lager mellan lokala och Azure. Detta innebär en SAP-systemet är antingen distribueras lokalt eller den har distribuerats i Azure. Du kan dock distribuera hello olika system för en SAP liggande i Azure eller lokalt. Du kan till exempel distribuera hello SAP CRM-utveckling och testning system i Azure som hello SAP CRM produktion system lokalt.
* Endast molnbaserad distribution: en distribution där hello Azure-prenumeration inte är ansluten via ett plats-till-plats eller ExpressRoute-anslutning toohello lokala nätverksinfrastruktur. Gemensam dokumentation för Azure dessa typer av distributioner beskrivs också som 'endast molnbaserad-distributioner. Virtuella datorer distribueras med den här metoden kan nås via hello internet och en offentlig IP-adress och/eller en offentlig DNS-namnet du tilldelade toohello virtuella datorer i Azure. För Microsoft Windows hello lokala Active Directory (AD) och DNS är inte utökat tooAzure i dessa typer av distributioner. Därför är hello virtuella datorer inte en del av hello lokala Active Directory. Detsamma gäller för Linux-implementeringar med till exempel OpenLDAP + Kerberos.

> [!NOTE]
> Endast molnbaserad distribution i det här dokumentet har definierats som slutförts SAP landskap kör enbart i Azure utan utökning av Active Directory / OpenLDAP eller namnmatchning från lokala i offentliga moln. Endast molnbaserad konfigurationer stöds inte för SAP produktionssystem eller konfigurationer där SAP STM eller andra lokala resurser måste toobe används mellan SAP-system som finns på Azure och resurser som finns lokalt.
>
>

* Anslutningar mellan lokala: Beskriver ett scenario där virtuella datorer som är distribuerade tooan Azure-prenumeration som har plats-till-plats, flera platser eller ExpressRoute-anslutning mellan hello lokalt datacenter(s) och Azure. Gemensamma Azure-dokumentationen dessa typer av distributioner beskrivs också som mellan lokala scenarier. hello orsaken till hello anslutning är tooextend lokala domänerna lokala Active Directory/OpenLDAP och lokala DNS i Azure. hello lokalt liggande är utökade toohello Azure tillgångar hello prenumeration. Med det här tillägget, kan hello virtuella datorer vara en del av hello lokala domän. Domänanvändare av hello lokal domän kan komma åt hello servrar och kan köra services på de virtuella datorer (till exempel DBMS services). Det är möjligt kommunikation och namnmatchning mellan virtuella datorer som distribueras på lokalt och distribuerade virtuella Azure-datorer. Detta är hello scenario planerar vi för de flesta SAP tillgångar toobe distribueras i. Mer information finns i [detta] [ vpn-gateway-cross-premises-options] artikel och [detta][vpn-gateway-site-to-site-create].

> [!NOTE]
> Anslutningar mellan lokala distributioner av SAP-system där Azure virtuella datorer som kör SAP-system är medlemmar i en lokal domän stöds för produktion SAP-system. Anslutningar mellan lokala konfigurationer stöds för att distribuera delar eller slutföra SAP landskap till Azure. Kör även hello fullständig SAP liggande i Azure kräver med dessa virtuella datorer som en del av lokal domän och Active Directory/OpenLDAP. I tidigare versioner av hello dokumentationen talat om Hybrid-IT-scenarier där hello termen ”Hybrid' är rotad i hello faktum att det finns en anslutning mellan Azure och lokalt. Dessutom hello faktum att hello virtuella datorer i Azure är en del av hello lokala Active Directory / OpenLDAP.
>
>

Vissa Microsoft-dokumentationen beskriver anslutningar mellan lokala scenarier lite annorlunda särskilt för DBMS HA konfigurationer. Hello gäller hello SAP-relaterade dokument handlar hello scenarion med flera lokaler bara toohaving ett plats-till-plats eller privat (ExpressRoute) anslutning och hello faktum att hello SAP liggande fördelas mellan lokala och Azure.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Resurser
hello är följande ytterligare guider tillgängliga för hello avsnittet SAP distributioner i Azure:

* [Azure virtuella datorer planering och implementering för SAP NetWeaver (det här dokumentet)][planning-guide]
* [Distribution av Azure virtuella datorer för SAP NetWeaver][deployment-guide]
* [Azure virtuella datorer DBMS-distribution för SAP NetWeaver][dbms-guide]

> [!IMPORTANT]
> Om det är möjligt en länk toohello hänvisar SAP installationsguiden används (referens InstGuide-01, se <http://service.sap.com/instguides>). När det gäller toohello förutsättningar och installationsprocessen hello SAP NetWeaver installationen guider ska alltid vara Läs noggrant, då det här dokumentet beskriver endast specifika uppgifter för SAP NetWeaver system som är installerade i en Microsoft Azure-dator.
>
>

hello följande SAP anteckningar är relaterade toohello avsnittet SAP i Azure:

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
| [2069760] |Oracle Linux 7.x SAP Installation och uppgradering |
| [1597355] |Växlingsutrymme rekommendation för Linux |

Också läsa hello [Tillståndsändringsavisering Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) som innehåller alla SAP anteckningar för Linux.

Allmän standardbegränsningar och maximala begränsningar i Azure-prenumerationer finns i [i den här artikeln][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>Möjliga scenarier
SAP visas ofta som en hello mest verksamhetskritiska program i företag. hello-arkitektur och drift av dessa program är främst mycket komplex och det är viktigt att se till att du uppfyller kraven på tillgänglighet och prestanda.

Företag har därför toothink noggrant om vilka program kan köras i en offentlig molnmiljö, oberoende av hello valt molntjänstleverantör.

Möjliga typer för att distribuera SAP NetWeaver baserat program i offentliga moln miljöer visas nedan:

1. Medelstora produktionssystem
2. Utvecklingssystem
3. Testa system
4. Prototyp system
5. Learning / Demonstration system

I ordning toosuccessfully distribuera SAP-system i Azure IaaS eller IaaS i allmänhet är det viktigt toounderstand hello betydande skillnader mellan hello erbjudanden för traditionella outsourcers eller värdar och IaaS erbjudanden. Medan hello traditionella värden eller outsourcer anpassas infrastruktur (nätverk, lagring och server-typ) toohello arbetsbelastning kunden toohost, utan i stället hello kundens ansvar toochoose hello rätt arbetsbelastningen för IaaS-distributioner.

Som ett första steg behöver kunderna tooverify hello följande objekt:

* hello SAP stöds VM typer av Azure
* hello SAP produkter/versioner som stöds på Azure
* hello OS och DBMS-versioner som stöds för hello specifika SAP släpper i Azure
* SAP-genomströmning som tillhandahålls av olika Azure-SKU: er

hello svar toothese frågor kan läsas i SAP-kommentar [1928533].

Som ett andra steg måste Azure-resurs och bandbredd begränsningar toobe jämfört med tooactual resursförbrukning lokalt system. Därför kan kunder behöver toobe bekant med hello olika funktioner i hello Azure typer som stöds med SAP under hello i:

* Processor och minne resurser av VM av olika typer och
* IOPS bandbredd på VM av olika typer och
* Nätverksfunktioner för VM av olika typer.

De flesta av dessa data kan hittas [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows].

Tänk på att hello begränsningarna i hello länken ovan är övre gräns. Det innebär inte att hello gränserna för någon av hello resurser, till exempel IOPS kan anges i alla fall. hello undantag är om hello CPU och minne resurser av typen valda VM. För hello VM typer som stöds av SAP, är hello CPU och minne resurser reserverade och som sådan är tillgängliga när som helst i tid för användning i hello VM.

hello Microsoft Azure-plattformen som andra plattformar, IaaS är flera innehavare. Det innebär att lagring, nätverk och andra resurser delas mellan klienter. Intelligent begränsning och kvot logik är används tooprevent en klient från att påverka hello prestanda för en annan klient (störningar granne) på drastiskt sätt. Även om logiken i Azure försöker tookeep avvikelser i bandbredd inträffade tenderar små, hög delade plattformar toointroduce större avvikelser i resursen/bandbreddstillgänglighet över många kunder har använt tooin sina lokala distributioner. Därför kan det uppstå olika nivåer av bandbredd om nätverks- eller lagringsinställningarna i/o (hello volymen samt svarstid) från minut toominute. hello sannolikheten att ett SAP-system på Azure kan uppleva större avvikelser än i ett lokalt system måste toobe beaktas.

Sista steget är tooevaluate tillgänglighet. Kan det hända, som hello underliggande Azure-infrastrukturen måste tooget uppdateras och kräver hello-värdar som kör virtuella datorer toobe startas om. I dessa fall skulle virtuella datorer som körs på värdarna stängs av och startas också. hello tidtagningen av sådana Underhåll görs under-core kontorstid för en viss region men hello potentiella fönstret några timmar under vilka en omstart kommer att ske är relativt bred. Det finns olika tekniker inom hello Azure-plattformen som kan vara konfigurerad toomitigate vissa eller alla hello påverkas av dessa uppdateringar. Framtida förbättringar av hello Azure-plattformen, DBMS och SAP-program är utformat toominimize hello effekten av sådana omstarter.

Toosuccessfully distribuera ett SAP-system på Azure, hello lokal SAP system operativsystem, databas, och SAP-program måste visas på hello SAP Azure stöd matrix ryms inom hello resurser hello Azure-infrastrukturen kan ge och vilka Arbeta med hello tillgänglighet serviceavtal för Microsoft Azure. Eftersom dessa system identifieras måste toodecide på något av följande två distributionsscenarier hello.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Endast molnbaserad - distributioner på virtuella datorer till Azure utan beroenden på hello lokalt kundens nätverk
![Enskild virtuell dator med SAP demo eller utbildning scenario distribueras i Azure][planning-guide-figure-100]

Det här scenariot är typiska för kurser eller demo-system, där alla hello komponenter för SAP och icke-program installeras på en enskild virtuell dator. SAP produktionssystem stöds inte i det här distributionsscenariot. I allmänhet uppfyller det här scenariot hello följande krav:

* hello själva de virtuella datorerna är tillgängliga via hello offentligt nätverk. Direkta nätverksanslutningar för hello-program som körs i hello VMs toohello lokalt nätverk på antingen hello företag äger hello demonstrationer eller utbildningar innehåll eller hello kunden är inte nödvändigt.
* Vid flera virtuella datorer som representerar hello utbildningar eller demo scenario, måste nätverket kommunikation och namnmatchning toowork mellan hello virtuella datorer. Men kommunikation mellan hello uppsättning virtuella datorer måste toobe isolerade så att flera uppsättningar med virtuella datorer kan distribueras sida vid sida utan störningar.  
* Internetanslutning krävs för hello slutanvändaren tooremote inloggning till toohello virtuella datorer finns i Azure. Beroende på hello gästoperativsystemet, Terminal Services/RDS eller VNC/ssh används tooaccess hello VM tooeither uppfylla hello utbildning uppgifter eller utföra hello demonstrationer. Om SAP-portarna som 3200, 3300 och 3600 kan också exponeras hello SAP programinstansen kan nås från alla anslutna Internet-skrivbordet.
* hello SAP system (och VM(s)) representerar ett fristående scenario i Azure som endast kräver en offentlig Internetanslutning för slutanvändarnas åtkomst och inte kräver en anslutning tooother virtuella datorer i Azure.
* SAPGUI och en webbläsare installeras och köras direkt på hello VM.
* Det krävs en snabb återställning av en VM toohello ursprungliga tillstånd och ny distribution av det ursprungliga tillståndet igen.
* Hello gäller demonstration och utbildning scenarier, som realiseras i flera virtuella datorer, en Active Directory / OpenLDAP och/eller DNS-tjänsten krävs för varje uppsättning virtuella datorer.

![Grupp med Virtuella datorer som representerar en demo eller utbildning scenariot i en Azure-molntjänst][planning-guide-figure-200]

Det är viktigt tookeep ihåg att hello virtuella datorer i varje hello anger behöver toobe distribueras parallellt, där hello VM-namn i varje hello set är hello samma.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Anslutningar mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med hello behovet av att fullständigt integrerat i hello lokalt nätverk
![VPN-anslutningar med plats-till-plats-anslutning (mellan platser)][planning-guide-figure-300]

Det här scenariot är en scenarion med flera lokaler med många möjlig distribution mönster. Det kan beskrivas så enkelt sätt som kör vissa delar av hello SAP liggande lokalt och andra delar av hello SAP liggande på Azure. Alla aspekter av hello fakta som en del av hello SAP komponenter körs på Azure ska vara transparent för slutanvändare. Därför Hej SAP Transport Correction System (STM), RFC kommunikation utskrift, säkerhet (till exempel SSO), etc. fungera sömlöst för hello SAP-system som körs på Azure. Men hello scenarion med flera lokaler beskriver också ett scenario där hello fullständig SAP liggande körs i Azure med hello kundens domänen och DNS utökats till Azure.

> [!NOTE]
> Detta är hello distributionsscenariot som stöds för att köra produktiva SAP-system.
>
>

Läs [i den här artikeln] [ vpn-gateway-create-site-to-site-rm-powershell] för mer information om hur tooconnect ditt lokala nätverk tooMicrosoft Azure

> [!IMPORTANT]
> När vi pratar om anslutningar mellan lokala scenarier mellan Azure och lokala kunddistributioner tittar vi hello Granulariteten för hela SAP-system. Scenarier som är *stöds inte* för anslutningar mellan lokala scenarier är:
>
> * Olika lager i SAP-program som körs i olika distributionsmetoder. Till exempel köra hello DBMS layer lokalt men hello SAP programnivå i virtuella datorer som distribueras som virtuella Azure-datorer eller tvärtom.
> * Vissa komponenter i ett SAP-lager i Azure och vissa lokalt. Till exempel dela instanser av hello SAP programnivå mellan lokala och virtuella Azure-datorer.
> * Distribution av virtuella datorer som kör SAP instanser av ett system över flera Azure-regioner stöds inte.
>
> hello orsaken till dessa begränsningar är hello krav för ett nätverk med mycket låg svarstid höga prestanda i en SAP-systemet, särskilt mellan hello programinstanser och hello DBMS lager av ett SAP-system.
>
>

### <a name="supported-os-and-database-releases"></a>OS- och databasen versioner som stöds
* Microsoft server-program stöds för virtuella Azure-tjänster ingår i den här artikeln: <http://support.microsoft.com/kb/2721672>.
* Operativsystem system-versioner som stöds, databasen system-versioner som stöds på virtuella Azure-tjänster tillsammans med SAP-program finns dokumenterade i SAP-kommentar [1928533].
* SAP-program och versioner som stöds på virtuella Azure-tjänster som finns dokumenterade i SAP-kommentar [1928533].
* Endast 64-bitars bilder är stöds toorun som gäst-VM i Azure för SAP-scenarier. Det innebär att endast 64-bitars SAP-program och databaser stöds.

## <a name="microsoft-azure-virtual-machine-services"></a>Virtuella Microsoft Azure-tjänster
hello Microsoft Azure-plattformen är ett internet-skala moln plattform finns och drivas på Microsoft data datacenter. hello-plattformen innehåller hello Microsoft Azure virtuella Services (infrastruktur som en tjänst eller IaaS) och en uppsättning omfattande plattform som en tjänst (PaaS)-funktioner.

hello Azure-plattformen minskar hello behovet för direkta teknik och infrastruktur inköp. Det förenklar underhåll och drift program genom att tillhandahålla på begäran beräkning och lagring toohost skala och hantera webbprogram och anslutna program. Infrastrukturhantering är automatisk med en plattform som är avsedd för hög tillgänglighet och dynamisk skalning toomatch användarbehov med hello alternativ med betalning per användning prismodellen.

![Placering av virtuella Microsoft Azure-tjänster][planning-guide-figure-400]

Med Azure Virtual Machine-tjänster, Microsoft aktiverar du toodeploy anpassade avbildningar tooAzure som IaaS instanser (se figur 4). hello virtuella datorer i Azure baseras på Hyper-V virtuella hårddiskar (VHD) och är kan toorun olika operativsystem som Gästoperativsystem.

Ur ett operativa upplevelser hello Azure Virtual Machine-tjänsten erbjuder liknande som virtuella datorer som distribuerats lokalt. Har dock hello betydande fördel som du inte behöver tooprocure, administrera och hantera hello infrastruktur. Utvecklare och administratörer har fullständig kontroll över hello operativsystemavbildningen inom dessa virtuella datorer. Administratörer kan logga in via fjärranslutning i toothose virtuella datorer tooperform underhåll och felsökning aktiviteter samt programvara distributionsåtgärder. Hello endast begränsningar finns i beaktande toodeployment hello storlekar och funktionerna i virtuella Azure-datorer. Dessa kanske inte lika bra detaljerade i konfigurationen som detta kan göras lokalt. Det finns ett val av VM-typer som representerar en kombination av:

* Antal vCPUs,
* Minne,
* Antal virtuella hårddiskar som kan kopplas
* Nätverk och lagring bandbredder.

hello storlek och begränsningar med olika storlekar för olika virtuella datorer som kan ses i en tabell i [(Linux) för den här artikeln] [ virtual-machines-sizes-linux] och [i den här artikeln (Windows)] [ virtual-machines-sizes-windows].

När du upptäcker, finns det olika familjer eller antal virtuella datorer. Du kan skilja hello efter familjer av virtuella datorer:

* A0 A7 VM typer: inte alla de som är certifierade för SAP. Första VM-serien som Azure IaaS fick introducerades i.
* A8 A11 VM typer: höga prestanda instanser. Körs på olika bättre utför beräkna värddatorer än andra A-series virtuella datorer.
* Dv2-D-serien VM typer: utför bättre än A0 A7. Inte alla typer av hello VM är certifierade med SAP.
* DS-DSv2-serien VM typer: liknande tooD/Dv2-serien, men finns kan tooconnect tooAzure Premium-lagring (finns i kapitlet [Azure Premium Storage] [ planning-guide-3.3.2] i det här dokumentet). Igen inte alla VM-typer som är certifierade med SAP.
* G-serien VM typer: höga minnesområdet VM-typer.
* GS-serien VM typer: som G-serien men inklusive hello alternativet toouse Azure Premium-lagring (finns i kapitlet [Azure Premium Storage] [ planning-guide-3.3.2] i det här dokumentet). När du använder GS-serien virtuella datorer som databasservrar, är det obligatoriska toouse Premium-lagring för DB data och transaktionen loggfiler

Du hittar hello samma processor och minne konfigurationer i annan VM-serien. Dock när du letar efter hello kapaciteten på dessa virtuella datorer utanför hello annan serie de skiljer sig avsevärt. Hej trots att ha samma konfiguration av CPU och minne. Orsaken är att hello underliggande värden servermaskinvara på hello införandet av hello VM av olika typer har olika dataflödesegenskaper.  Vanligtvis hello skillnaden visas i kapaciteten också avspeglas i hello pris hello olika virtuella datorer.

Inte alla olika VM-serien kan erbjudas i var och en av hello Azure-regioner (för Azure-regioner finns nästa kapitel). Tänk också att inte alla virtuella datorer eller VM-serien är certifierad för SAP.

> [!IMPORTANT]
> Hello användning av SAP NetWeaver baserat program, endast hello delmängd av VM-typer och konfigurationer som anges i SAP-kommentar [1928533] stöds.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-regioner
Microsoft kan toodeploy virtuella datorer i så kallade ”Azure-regioner”. En Azure-Region kanske en eller flera datacenter som befinner sig i närheten. För de flesta hello geopolitiska regioner i hello world Microsoft har minst två Azure-regioner. Till exempel i Europa har en Azure-Region 'Nordeuropa, och ett av 'Västeuropa'. Dessa två Azure-regioner inom en geopolitiska region avgränsas med tillräckligt betydande avstånd så att naturliga eller tekniska katastrofer inte påverkar både Azure-regioner i hello samma geopolitiska region. Eftersom Microsoft bygger stadigt ut nya Azure-regioner i olika geopolitiska regioner globalt, växande ständigt hello antalet dessa regioner och från och med Dec 2015 uppnått hello antalet 20 Azure-regioner med ytterligare områden som redan har presenterat. I alla dessa regioner, inklusive hello två Azure-regioner i Kina kan du som kund distribuera SAP-system. Aktuella uppdaterad information om Azure-regioner finns i den här webbplatsen: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>hello Microsoft Azure Virtual Machine-begrepp
Microsoft Azure tillhandahåller en infrastruktur som en tjänst (IaaS) lösning toohost virtuella datorer med liknande funktioner som ett lokalt virtualiseringslösning. Du kan kan toocreate virtuella datorer från inom hello Azure-portalen PowerShell eller CLI, som också ger distribution och hanteringsfunktioner.

Azure Resource Manager kan du tooprovision dina program med en deklarativ mall. I samma mall kan du distribuera flera tjänster tillsammans med deras beroenden. Du använder hello samma mall toorepeatedly distribuera programmet under varje fas av hello livscykel för programmet.

Mer information om hur du använder Resource Manager-mallar finns här:

* [Distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och hello Azure CLI] [.. /.. / linux/create-ssh-secured-vm-from-template.md]
* [Hantera virtuella datorer med Azure Resource Manager och PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/Documentation/Templates/>

En annan intressant funktion är hello möjlighet toocreate bilder från virtuella datorer, vilket gör att du tooprepare vissa databaser som du använder kan tooquickly distribuera instanser för virtuella datorer som uppfyller dina krav.

Mer information om hur du skapar bilder från virtuella datorer finns i [(Linux) för den här artikeln] [ virtual-machines-linux-capture-image-resource-manager] och [i den här artikeln (Windows)][virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Feldomäner
Feldomäner representerar en fysisk enhet skulle misslyckas, mycket nära relaterade toohello fysisk infrastruktur som används i datacenter och när en fysisk bladet eller rack kan betraktas som en Feldomän finns det ingen direkt mappningen mellan hello två.

När du distribuerar flera virtuella datorer som en del av en SAP-system i virtuella Microsoft Azure-tjänster, kan du påverka hello Azure-Infrastrukturkontrollanten toodeploy ditt program till olika Feldomäner, vilket uppfyller hello hello krav Microsoft Azure-serviceavtalet. Dock hello distribution av Feldomäner över en Azure-Skalningsenhet (insamling av hundratals datornoder eller lagringsnoder och nätverk) eller hello tilldelningen av virtuella datorer tooa specifika Feldomän är något som du inte har direkt kontroll. I ordning toodirect hello Azure fabric controller toodeploy en uppsättning virtuella datorer över olika Feldomäner måste tooassign en Tillgänglighetsuppsättning för Azure-toohello VMs vid tidpunkten för distribution. Mer information om Azure-Tillgänglighetsuppsättningar finns i kapitlet [Azure-Tillgänglighetsuppsättningar] [ planning-guide-3.2.3] i det här dokumentet.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Uppgradera domäner
Uppgraderingsdomäner representerar en logisk enhet som hjälper toodetermine hur en virtuell dator i en SAP-system som består av SAP-instanser som körs i flera virtuella datorer uppdateras. När en uppgradering sker genomgår Microsoft Azure hello för att uppdatera dessa Uppgraderingsdomäner i taget. Genom att sprida VMs vid tidpunkten för distribution över olika uppgradera domäner, kan du skydda SAP systemet delvis från potentiell nedtid. I ordning tooforce Azure toodeploy hello virtuella datorer i ett SAP-system som är utspridda över olika uppgradera domäner måste tooset ett specifikt attribut vid tidpunkten för distribution av varje virtuell dator. Liknande tooFault domäner, en Skalningsenhet för Azure är indelade i flera uppgradera domäner. I ordning toodirect hello Azure fabric controller toodeploy en uppsättning virtuella datorer över olika uppgradera domäner behöver du tooassign en Tillgänglighetsuppsättning för Azure-toohello VMs vid tidpunkten för distribution. Mer information om Azure-Tillgänglighetsuppsättningar finns i kapitlet [Azure-Tillgänglighetsuppsättningar] [ planning-guide-3.2.3] nedan.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure Tillgänglighetsuppsättningar
Azure virtuella datorer i en Tillgänglighetsuppsättning av typen Azure distribueras av hello Azure-Infrastrukturkontrollanten över olika fel- och uppgradera domäner. hello syftar hello distribution över olika fel- och uppgradera domäner tooprevent alla virtuella datorer i en SAP-system från att stänga av i hello fall av infrastrukturens underhåll eller ett fel i en Feldomän. Som standard ingår virtuella datorer inte i en Tillgänglighetsuppsättning. hello deltagande i en virtuell dator i en Tillgänglighetsuppsättning har definierats vid tidpunkten för distribution eller vid ett senare tillfälle genom en omkonfiguration och en ny distribution av en virtuell dator.

toounderstand hello begreppet Azure-Tillgänglighetsuppsättningar och hello sätt Tillgänglighetsuppsättningar relaterar tooFault och uppgradera domäner läsa [i den här artikeln][virtual-machines-manage-availability]

toodefine tillgänglighetsuppsättningar för ARM via en json-mall finns [hello rest-api-specifikationerna](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) och Sök efter ”tillgänglighet”.

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Lagring: Microsoft Azure Storage- och Datadiskar
Microsoft Azure virtuella datorer använda olika lagringstyper. När du implementerar SAP på virtuella Azure-tjänster, är det viktigt toounderstand hello skillnaderna mellan dessa två typer av lagring:

* Icke-beständig, temporär lagring.
* Beständig lagring.

hello icke-beständig lagring är direkt anslutna toohello virtuella datorer som körs och finns på hello datornoderna själva – hello lokala instance store (tillfällig lagring). hello beroende på hello storleken på hello virtuella valt när hello distributionen har startats. Den här lagringstypen är temporär och därför hello disken initieras när en virtuell dator-instansen startas om. Hello växlingsfil för hello operativsystemet finns normalt på tillfälliga disken.

- - -
> ![Windows][Logo_Windows] Windows
>
> Hello temp enhet är monterad enhet D:\ i en distribuerad virtuell dator på virtuella Windows-datorer.
>
> ![Linux][Logo_Linux] Linux
>
> Den är monterad som /mnt/resource eller /mnt på Linux virtuella datorer. Se mer information här:
>
> * [Hur tooAttach en datadisk tooa virtuell Linux-dator][virtual-machines-linux-how-to-attach-disk]
> * <https://docs.microsoft.com/Azure/Storage/Storage-About-Disks-and-vhds-Linux#Temporary-disk>
>
>

- - -
hello faktiska enhet är beständig eftersom den hämtar lagras på värdservern för hello sig själv. Om hello VM flyttas i en omdistribution (till exempel på grund av toomaintenance på hello värden eller avstängning och omstart) hello innehållet i hello enheten går förlorad. Det är därför inte en alternativet toostore viktiga data på den här enheten. hello typ av media som används för den här typen av lagring skiljer sig mellan olika virtuella dator-serien med mycket olika prestandaegenskaper som från och med juni 2015 ser ut som:

* A5 A7: Mycket begränsad prestanda. Rekommenderas inte för något utöver växlingsfilen
* A8-A11: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.
* D-serien: Goda prestandaegenskaper med vissa sedan tusen IOPS och > 1 GBIT/s genomströmning.
* DS-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.
* G-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.
* GS-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning.

Uttryck ovanför kopplar toohello VM typer som är certifierade med SAP. hello VM-serien med utmärkt IOPS och genomströmning uppfyller kraven för att dra nytta av vissa DBMS-funktioner. Mer information finns i hello [DBMS Deployment Guide][dbms-guide].

Microsoft Azure Storage tillhandahåller beständiga lagring och hello vanliga nivåer av skydd och redundans på SAN-lagring. Diskar baserat på Azure Storage är en virtuell hårddisk (VHD) finns i hello Azure Storage-tjänster. hello lokala OS-Disk (C: Windows\, Linux/dev/sda1) lagras på hello Azure Storage, och ytterligare volymer/diskar monterade toohello VM lagras där, för.

Det är möjligt tooupload en befintlig virtuell Hårddisk från lokala eller skapar en tom från i Azure och koppla dessa toodeployed virtuella datorer.

När du skapar eller ladda upp en VHD till Azure Storage är möjliga toomount och koppla de befintliga virtuella datorn och toocopy befintliga (omonterade) VHD tooan.

Data och ändringar i de är säkra när startas om och återskapa en virtuell datorinstans som de virtuella hårddiskarna är beständiga. Även om en instans har tagits bort kan hålla dig säker dessa virtuella hårddiskar och kan distribueras eller om icke-OS-diskar kan vara monterade tooother virtuella datorer.

Nätverk med Azure Storage redundans olika nivåer inom hello kan konfigureras:

* Lägsta nivå som kan väljas är 'lokala redundans', vilket är likvärdiga toothree-replik av hello data i hello samma Datacenter för en Azure-Region (finns i kapitlet [Azure-regioner][planning-guide-3.1]).
* Zonen redundant lagring, och vilka uppslag hello tre bilder över olika datacenter inom hello samma Azure-Region.
* Standardnivån för redundans är geografisk redundans, vilket replikerar asynkront hello innehåll till en annan tre avbildningar av hello data till en annan Azure-Region som är värd för hello samma geopolitiska region.

Se även hello tabellen på den här artikeln om hello olika redundansalternativ: <https://azure.microsoft.com/pricing/details/storage/>

Mer information om Azure Storage finns här:

* <https://Azure.microsoft.com/Documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/site-Recovery>
* <https://docs.microsoft.com/REST/API/storageservices/Understanding-block-BLOBs--append-BLOBs--and-Page-BLOBs>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-disk-Encryption-for-Linux-and-Windows-Virtual-Machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Azure standardlagring
Azure standardlagring var hello typ av lagring som är tillgängliga när Azure IaaS släpptes. Det fanns IOPS kvoter tillämpas per enskild disk. Latens som uppstod var inte i hello samma klass som SAN/NAS-enheter normalt distribueras för avancerade SAP-datorer lagras lokalt. Dock hello Azure standardlagring visat sig vara tillräcklig för flera hundra SAP-system under tiden distribueras i Azure.

Diskar som är lagrade på Azure-Standard Lagringskonton debiteras baserat på hello faktiska data som lagras, hello mängden lagringstransaktioner och utgående dataöverföringar redundans-alternativ som valts. Många diskar kan skapas på hello högst 1 TB i storlek, men så länge de förblir tom är kostnadsfri. Om du fyller sedan en virtuell Hårddisk med 100GB, debiteras du för att lagra 100GB och inte för hello nominell storlek hello VHD återställningsjobb skapades med.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium-lagring
I April 2015 introducerade Microsoft Azure Premium-lagring. Premium-lagring har fått introducerades med hello målet tooprovide:

* Bättre i/o-svarstid.
* Bättre genomflöde.
* Mindre variationer i i/o-svarstid.

För detta ändamål introducerades många ändringar av vilka hello två viktigaste är:

* Användning av SSD-diskar i hello Azure Storage-noder
* En ny läsningscachens som backas upp av hello lokala SSD på ett Azure compute-nod

I motsatt tooStandard storage där funktioner inte ändras beroende på hello storlek hello disk (VHD eller), Premium-lagring har tre olika disk kategorier som visas i den här artikeln: <https://azure.microsoft.com/ prissättning/uppgifter / / ohanterad-lagringsdiskar />

Du ser att IOPS-disk och disk genomströmning eller disken är beroende av hello storlek kategori hello diskar

Kostnad bas hello gäller Premium-lagring är inte hello faktiska datavolym lagras i dessa diskar, men hello storlek kategori av en disk, oberoende av hello mängden hello data som lagras i hello disk.

Du kan också skapa diskar på Premium-lagring som inte direkt mappning till hello storlek kategorier som visas. Detta kan vara fallet hello, särskilt när du kopierar diskar från standardlagring till Premium-lagring. I sådana fall utförs en mappning toohello nästa största Premium-lagring diskalternativet.

Tänk på att bara vissa VM-serien kan dra nytta av hello Azure Premium-lagring. Från och med Dec 2015 är dessa hello DS - och GS-serien. hello DS-serien är i princip hello samma som D-serien med hello undantaget att DS-serien har hello möjlighet toomount Premium-lagring-baserade virtuella datorer dessutom toodisks som finns på Azure standardlagring. Samma sak gäller för G-serien jämfört med tooGS-serien.

Om du checkar ut hello tillhör hello DS-serien virtuella datorer i [(Linux) för den här artikeln] [ virtual-machines-sizes-linux] och [i den här artikeln (Windows)][virtual-machines-sizes-windows], inser du som det finns data volym begränsningar tooPremium diskar med lagringsutrymme på hello Granulariteten för hello VM-nivå. Olika DS-serien eller GS-serien virtuella datorer måste du också ha olika begränsningar i angående toohello antalet datadiskar som kan monteras. Dessa gränser dokumenteras i hello artikeln som nämns ovan samt. Men i princip betyder att om dig, till exempel montera 32 x P30 diskar tooa enda DS14 virtuell dator inte kan du få 32 x hello maximalt dataflöde för en P30 disk. I stället hello maximalt dataflöde på VM-nivå enligt beskrivningen i hello artikel gränser datagenomströmning.

Mer information om Premium-lagring finns här: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Hanterade diskar
Hanterade diskar är en ny resurstyp i Azure Resource Manager som kan användas i stället för virtuella hårddiskar som lagras i Azure Storage-konton. Hanterade diskar justeras automatiskt med hello Tillgänglighetsuppsättning hello virtuell dator som de är anslutna tooand därför öka hello tillgängligheten för virtuella datorer och hello-tjänster som körs på hello virtuella datorn. Mer information finns i hello [översiktsartikel](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Vi rekommenderar tooyou Använd hanterade disken, eftersom de förenkla hello distribution och hantering av virtuella datorer.
SAP stöder för närvarande endast hanteras Premiumdiskar. Mer information finns i SAP-kommentar [1928533].

#### <a name="azure-storage-accounts"></a>Azure Storage-konton
När du distribuerar tjänster eller virtuella datorer i Azure, kan distribution av virtuella hårddiskar och VM-avbildningar ordnas i enheter som kallas Azure Storage-konton. När du planerar en Azure-distribution måste toocarefully överväga hello begränsningar i Azure. På ena sidan av hello finns ett begränsat antal Lagringskonton per Azure-prenumeration. Varje Azure Storage-konto kan innehålla ett stort antal VHD-filer, men det finns en fast gräns på hello Totalt antal IOPS per Lagringskonto. När du distribuerar hundratals SAP virtuella datorer med DBMS system skapar betydande i/o-anrop, är det rekommenderade toodistribute hög IOPS DBMS VMs mellan flera Azure Storage-konton. Vara måste försiktig inte tooexceed hello aktuella begränsningen på Azure Storage-konton per prenumeration. Eftersom lagringen är en viktig del av hello databasen distributionen för en SAP-system, detta begrepp beskrivs i detalj i hello redan refereras till [DBMS Deployment Guide][dbms-guide].

Mer information om Azure Storage-konton finns i [i den här artikeln][storage-scalability-targets]. Läsa den här artikeln, inser du att det finns skillnader i hello begränsningar mellan Lagringskonton i Azure-Standard och Premium-Lagringskonton. Viktiga skillnader är hello mängden data som kan lagras i ett Lagringskonto. I standardlagring är hello volym en omfattning som är större än med Premium-lagring. På hello är andra sidan hello standardkonto för lagring kraftigt begränsad IOPS (se kolumnen 'Totala begära hastigheten'), medan hello Azure Premium Storage-konto har ingen sådan begränsning. Diskuteras information och resultatet av dessa skillnader när du ska diskutera hello distributioner av SAP-system, särskilt hello DBMS-servrar.

Ha hello möjligheten toocreate olika behållare i hello syftet att ordna och kategorisera olika virtuella hårddiskar inom ett Lagringskonto. Dessa behållare används vanligtvis för att till exempel separata virtuella hårddiskar på olika virtuella datorer. Det finns inga konsekvenser för prestanda i med hjälp av en behållare eller flera behållare under en enda Azure Storage-konto.

I Azure följer en VHD-namnet hello följande namngivning anslutning som måste tooprovide ett unikt namn för hello VHD i Azure:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Som nämnts hello sträng ovan måste toouniquely identifiera hello VHD som lagras på Azure Storage.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure-nätverk
Microsoft Azure tillhandahåller nätverksinfrastruktur, vilket gör att hello mappningen av alla scenarier som vi vill toorealize med SAP-program. hello funktioner är:

* Åtkomst från hello utanför direkt toohello virtuella datorer via Windows Terminal Services eller ssh/VNC
* Åtkomst tooservices och specifika portar som används av program inom hello virtuella datorer
* Intern kommunikation och namnmatchning mellan en grupp med virtuella datorer som distribueras som virtuella Azure-datorer
* Anslutning mellan en kund lokala nätverk och hello Azure-nätverk
* Mellan Azure-Region eller data center anslutning mellan Azure platser

Mer information hittar du här: <https://azure.microsoft.com/documentation/services/virtual-network/>

Det finns många olika möjligheter tooconfigure namn och IP-lösning i Azure. I detta dokument, endast molnbaserad scenarier som förlitar sig på hello standardvärdet med hjälp av Azure DNS (i kontrast toodefining en egen DNS-tjänst). Det finns också en ny Azure DNS-tjänst som kan användas i stället för att ställa in DNS-servern. Mer information finns i [i den här artikeln] [ virtual-networks-manage-dns-in-vnet] på [den här sidan](https://azure.microsoft.com/services/dns/).

För anslutningar mellan lokala scenarier vi måste hello faktum att hello lokala DNS-AD/OpenLDAP har utökats via VPN eller privat anslutning tooAzure. För vissa scenarier som beskrivs här, kan det vara nödvändigt toohave replik av en AD/OpenLDAP installerad i Azure.

Eftersom nätverk och namnmatchning är en viktig del av hello databasen distributionen för en SAP-system, detta begrepp som diskuteras i detalj i hello [DBMS Deployment Guide][dbms-guide].

##### <a name="azure-virtual-networks"></a>Azure Virtual Networks
Du kan definiera hello adressintervallet hello privata IP-adresser som allokerats av Azure DHCP-funktioner genom att bygga upp ett Azure Virtual Network. I scenarier med anslutningar mellan lokala fördelas hello IP-adressintervall som definierats fortfarande med hjälp av DHCP av Azure. Domain Name resolution görs lokalt (förutsatt att hello virtuella datorer är en del av en lokal domän) och därför kan matcha adresser utöver olika Azure-molntjänster.

Varje virtuell dator i Azure måste toobe ansluten tooa virtuellt nätverk.

Mer information finns i [i den här artikeln] [ resource-groups-networking] på [den här sidan](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd TODO gick inte att hitta en artikel som innehåller hello OpenLDAP avsnittet + ARM;)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Som standard när en virtuell dator distribueras ändra du inte hello konfiguration av virtuellt nätverk. hello TCP/IP-inställningar måste finnas kvar toohello Azure DHCP-server. Standardbeteendet är dynamisk IP-tilldelning.
>
>

hello MAC-adressen för hello virtuella nätverkskort kan ändras, till exempel efter ändring av storlek och Hej Windows eller Linux gästoperativsystemet hämtar hello nytt nätverkskort och använder automatiskt DHCP tooassign hello IP- och DNS-adresser i det här fallet.

##### <a name="static-ip-assignment"></a>Statiska IP-tilldelning
Det är möjligt tooassign fast eller reserverade IP-adresser tooVMs inom ett Azure Virtual Network. Hello virtuella datorer som körs i ett Azure Virtual Network öppnas en bra möjlighet tooleverage den här funktionen om behövs eller krävs för vissa scenarier. hello IP-adresstilldelning förblir giltigt i hela hello förekomsten av hello VM, oberoende av om hello VM körs eller avstängning. Därför behöver du tootake hello totala antalet virtuella datorer (VM körs och stoppas) i beräkningen när du definierar hello intervall med IP-adresser för hello virtuellt nätverk. hello IP-adressen förblir tilldelade tills hello VM och dess nätverksgränssnittet tas bort eller tills hello IP-adressen hämtar Frigör igen. Mer information finns [i den här artikeln][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Flera nätverkskort per VM
Du kan definiera flera virtuella nätverkskort (vNIC) för en virtuell dator i Azure. Med hello möjlighet toohave flera vNICs kan du starta tooset upp nätverket trafik avgränsning om exempelvis klienttrafik dirigeras via ett virtuellt nätverkskort och backend-trafik dirigeras via en andra vNIC. Beroende på hello typ av VM finns olika begränsningar i fråga om toohello antal vNICs. Exakta detaljer, funktioner och begränsningar finns i följande artiklar:

* [Skapa en virtuell Windows-dator med flera nätverkskort][virtual-networks-multiple-nics-windows]
* [Skapa en Linux VM med flera nätverkskort][virtual-networks-multiple-nics-linux]
* [Distribuera flera NIC virtuella datorer med hjälp av en mall][virtual-network-deploy-multinic-arm-template]
* [Distribuera flera NIC virtuella datorer med hjälp av PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Distribuera flera NIC virtuella datorer med hello Azure CLI][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Plats-till-plats-anslutning
Anslutningar mellan lokala är virtuella Azure-datorer och lokala kopplad till en transparent och permanenta VPN-anslutning. Är det förväntade toobecome hello vanligaste SAP-distribution mönster i Azure. hello antas att operativa procedurer och processer med SAP-instanser i Azure bör fungera transparent. Det innebär att du ska vara kan tooprint utanför dessa system som använder hello SAP Transport Management System (Proportionerlig) tootransport ändras från en utvecklingssystemet i Azure tooa testsystemet som är distribuerade lokalt. Mer dokumentation runt plats-till-plats finns i [i den här artikeln][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-Tunnel-enhet
I ordning toocreate en plats-till-plats-anslutning (lokala data center tooAzure datacenter), måste du tooeither hämtar och konfigurera en VPN-enhet eller använda Routing and Remote Access Service (RRAS) som introducerades som en programvarukomponent med Windows Server 2012.

* [Skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning med hjälp av PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Om VPN-enheter för plats-till-plats VPN-Gateway-anslutningar][vpn-gateway-about-vpn-devices]
* [Vanliga frågor och svar för VPN-Gateway][vpn-gateway-vpn-faq]

![Plats-till-plats-anslutning mellan Azure och lokalt][planning-guide-figure-600]

hello ovanstående bild visar två Azure-prenumerationer har IP-adressen underintervall reserverade för användning i virtuella nätverk i Azure. hello anslutningen från hello lokalt nätverk tooAzure upprättas via VPN.

#### <a name="point-to-site-vpn"></a>Punkt-till-plats VPN
Punkt-till-plats VPN kräver varje klient datorn tooconnect med sin egen VPN till Azure. Punkt-till-plats-anslutning är inte praktiskt hello SAP scenarier vi tittar på. Därför kan hänvisningar inga ytterligare toopoint-till-plats VPN-anslutning.

Mer information hittar du här
* [Konfigurera en punkt-till-plats-anslutning tooa VNet med hjälp av hello Azure-portalen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Konfigurera en punkt-till-plats-anslutning tooa VNet med PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>Flera plats-VPN
Azure erbjuder också Nuförtiden hello möjligheten toocreate flera plats-VPN-anslutning för en Azure-prenumeration. Tidigare var en enda prenumeration begränsad tooone plats-till-plats VPN-anslutning. Den här begränsningen försvann med flera plats-VPN-anslutningar för en enda prenumeration. Detta gör det möjligt tooleverage mer än en Azure-Region för en viss prenumeration via anslutningar mellan lokala konfigurationer.

Mer dokumentation finns [i den här artikeln][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (MShermannd TODO hittades ingen ARM doku länk)

#### <a name="vnet-toovnet-connection"></a>VNet tooVNet anslutning
Använda flera plats-VPN måste tooconfigure ett separat virtuellt nätverk med Azure i varje hello regioner. Du har dock ofta hello kravet att hello programvarukomponenter i hello olika områden ska kommunicera med varandra. Den här kommunikationen ska helst inte vidarebefordras från en Azure-Region tooon lokalt och från toohello andra Azure-Region. tooshortcut, Azure erbjuder hello möjligheten tooconfigure en anslutning från ett virtuellt Azure-nätverk i en region tooanother Azure-nätverk finns i en annan region. Den här funktionen kallas VNet-till-VNet-anslutningen. Mer information om den här funktionen finns här: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-tooazure--expressroute"></a>Privata anslutning tooAzure – ExpressRoute
Microsoft Azure ExpressRoute kan privata anslutningar mellan Azure-datacenter och antingen hello kundens lokala infrastruktur eller i en miljö med samplacering hello skapas. ExpressRoute erbjuds av olika MPLS (paketväxlat) VPN-leverantörer eller andra leverantörer. ExpressRoute-anslutningar inte överskrider hello offentliga Internet. ExpressRoute-anslutningar ger högre säkerhet och högre tillförlitlighet via flera parallella kretsar högre hastighet och lägre svarstider än vanliga anslutningar över hello Internet.

Hitta mer information om Azure ExpressRoute och erbjudanden här:

* <https://Azure.microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-faqs/>

Expressroute gör att flera Azure-prenumerationer via en ExpressRoute-krets dokumenteras här

* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-Circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Tvingad tunneltrafik vid mellan platser
För virtuella datorer ansluta till lokala domäner via plats-till-plats, punkt-till-plats eller ExpressRoute, behöver du till att hello Internet-proxyinställningarna komma att distribueras för alla hello användare i dessa virtuella datorer samt toomake. Som standard program som körs i de virtuella datorer eller användare som använder en webbläsare tooaccess hello internet inte skulle gå igenom hello företagets proxy, men skulle ansluta direkt till Azure toohello internet. Men även hello proxyinställning är inte en 100-procentig toodirect hello trafik via hello företagets proxy eftersom det är program och tjänster toocheck ansvar för hello-proxy. Om program som körs i hello VM inte gör detta eller administratör manipulerar hello inställningar, trafik toohello Internet detoured med igen direkt via Azure toohello Internet.

I ordning tooavoid detta, kan du konfigurera Tvingad Tunneling med plats-till-plats-anslutning mellan Azure och lokalt. hello detaljerad beskrivning av hello Tvingad tunneltrafik funktionen publiceras här <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Tvingad Tunneling med ExpressRoute aktiveras av kunder annonserar en standardväg via hello ExpressRoute BGP peeringsessioner.

#### <a name="summary-of-azure-networking"></a>Översikt över Azure-nätverk
Det här kapitlet innehåller många viktiga punkter om Azure-nätverk. Här följer en sammanfattning av hello huvudpunkter:

* Virtuella Azure-nätverk kan tooset hello nätverk enligt tooyour egna behov
* Virtuella Azure-nätverk kan balanserad tooassign IP-adress, intervall tooVMs eller tilldela fasta IP-adresser tooVMs
* tooset upp en plats-till-plats eller en punkt-till-plats-anslutning toocreate ett Azure Virtual Network måste först
* När en virtuell dator har distribuerats, är det inte längre möjligt toochange hello virtuellt nätverk tilldelas toohello VM

### <a name="quotas-in-azure-virtual-machine-services"></a>Kvoter i virtuell dator i Azure-tjänster
Vi behöver toobe Rensa om hello faktum att hello lagring och nätverksinfrastruktur delas mellan virtuella datorer som kör en mängd olika tjänster i hello Azure-infrastrukturen. Och precis som hello kundens eget datacenter, överbelasta av hello infrastruktur resurser tar plats tooa grad. hello Microsoft Azure-plattformen använder disk, CPU, nätverk och andra kvoter toolimit hello resursförbrukning och toopreserve konsekvent och deterministisk prestanda.  hello VM av olika typer (A5, A6 osv.) har olika kvoter för hello antal diskar, processor, RAM-minne och nätverk.

> [!NOTE]
> Processor och minne resurser av hello VM typer som stöds av SAP är förallokerade på hello värdnoderna. Det innebär att hello resurser på hello värden är tillgängliga som definieras av hello VM typen när hello VM har distribuerats.
>
>

När planering och storleksanpassa SAP på Azure lösningar hello kvoter för varje virtuell datorstorlek måste beaktas. hello VM kvoter beskrivs [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows].

hello kvoter beskrivs representerar hello teoretisk högsta värden.  hello gränsen på IOPS per disk kan uppnås med små IOs (8kb), men de kan eventuellt inte uppnås med stora IOs (1Mb).  hello IOPS gränsen tillämpas på hello Granulariteten för enskild disk.

Som en grov beslut trädet toodecide kan om ett SAP-system som passar in i Azure-tjänster för virtuella datorer och dess funktioner eller om ett befintligt system måste konfigureras på olika sätt i ordning toodeploy hello system på Azure toobe, hello beslutsträdet nedan användas:

![Decision trädet toodecide möjlighet toodeploy SAP på Azure][planning-guide-figure-700]

**Steg 1**: hello viktigaste information toostart med är hello SAP krav för ett visst SAP-system. hello SAP krav måste toobe separeras i hello DBMS och hello SAP programmet del, även om hello SAP system redan distribuerats lokalt i en nivå 2-konfiguration. Hello SAP relaterade toohello maskinvara används ofta kan fastställa eller beräknat baserat på befintliga SAP prestandamått för befintliga system. hello resultat finns här: <http://global.sap.com/campaigns/benchmark/index.epx>.
För nyligen distribuerade SAP-system, bör du har gått igenom en storlek Övning som bestämmer hello SAP krav hello system.
Se även den här bloggen och bifogade dokumentet för SAP storlek på Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Steg 2**: befintliga system hello i i/o-volym och mäts i/o-åtgärder per sekund på hello DBMS-servern. Nyligen planerad system ge hello storleksanpassa Övning för hello nya system även grov idéer hello i/o-krav på hello DBMS-sida. Om du är osäker, måste du slutligen tooconduct ett konceptbevis.

**Steg 3**: jämföra hello SAP krav för hello DBMS-server med hello SAP hello VM av olika typer av Azure kan ge. hello information på SAP hello olika Virtuella Azure-typer finns i SAP-kommentar [1928533]. hello fokus bör finnas på hello DBMS VM först eftersom hello databasen lagret är hello lager i en SAP NetWeaver system som inte kan skaländras ut i hello flesta distributioner. Däremot kan hello SAP programnivå skalas ut. Om ingen av hello SAP stöds Virtuella Azure-typer kan leverera hello krävs SAP, hello arbetsbelastning hello planerad SAP-system kan inte köras på Azure. Du måste antingen toodeploy hello system lokalt eller du behöver toochange hello arbetsbelastning volym för hello system.

**Steg 4**: enligt [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows], Azure tillämpar en kvot för IOPS per disk oberoende om du använder standardlagring eller Premium-lagring. Beroende på hello VM-typ, hello antalet datadiskar som kan monteras varierar. Därför kan du beräkna IOPS maximalt som kan uppnås med respektive hello VM av olika typer. Beroende på hello databasen filens layout, du kan stripe-diskar toobecome en volym i hello gäst-OS. Men om hello IOPS volym i en distribuerad SAP-systemet överskrider hello beräknade gränserna för hello största VM typ av Azure och om det finns inga chansen toocompensate med mer minne, kan hello arbetsbelastning hello SAP-system påverkas allvarligt. I sådana fall måste nådde du en plats där du inte bör distribuera hello system på Azure.

**Steg 5**: särskilt i SAP-system som distribueras lokalt i nivå 2-konfigurationer, hello risken är hello system behöva toobe som konfigurerats i Azure i en nivå 3-konfiguration. I det här steget måste toocheck om det finns en komponent i hello SAP programnivå, vilket inte skalas ut och som inte passar in i hello CPU och minne resurser hello annat Azure VM typer erbjudande. Om det verkligen är en sådan komponent, kan inte hello SAP-systemet och dess arbetsbelastningen distribueras till Azure. Men om du kan skala ut hello SAP-programkomponenter i flera virtuella Azure-datorer, hello system kan distribueras till Azure.

**Steg 6**: om hello DBMS och SAP layer programkomponenter kan köras på virtuella Azure-datorer, hello konfiguration måste toobe som definierats med avseende på:

* Antal virtuella Azure-datorer
* VM-typer för hello enskilda komponenter
* Antal virtuella hårddiskar i DBMS VM tooprovide tillräckligt med IOPS

## <a name="managing-azure-assets"></a>Hantera Azure tillgångar
### <a name="azure-portal"></a>Azure Portal
hello Azure-portalen är ett av tre gränssnitt toomanage Virtuella Azure-distributioner. hello grundläggande hanteringsuppgifter som att distribuera virtuella datorer från bilder, kan göras via hello Azure-portalen. Dessutom hello skapande av Lagringskonton, virtuella nätverk och andra Azure-komponenter är också uppgifter hello Azure-portalen kan hantera mycket bra. Funktioner som överför virtuella hårddiskar från lokala tooAzure eller kopierar en VHD i Azure är dock uppgifter som kräver verktyg från tredje part eller administration via PowerShell eller CLI.

![Microsoft Azure portal – översikt över virtuella datorer][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Uppgifter för administration och konfiguration för hello virtuella instansen kan utföras från inom hello Azure-portalen.

Förutom att starta om och stänga av en virtuell dator kan du också koppla, koppla från och skapa datadiskar för hello virtuella instans toocapture hello-instans för förberedelse av avbildningen och konfigurera hello storleken på hello virtuell datorinstans.

hello Azure-portalen tillhandahåller grundläggande funktioner toodeploy och konfigurera virtuella datorer och många andra Azure-tjänster. Men inte alla tillgängliga funktioner omfattas av hello Azure-portalen. Hello Azure-portalen är det inte möjligt tooperform uppgifter som:

* Överför tooAzure för virtuella hårddiskar
* Kopiera virtuella datorer

[comment]: <> (MShermannd TODO vad om automatisering i tjänsten för SAP virtuella datorer?)
[comment]: <> (MSSedusch distribution av flera virtuella datorer os under tiden möjligt)
[comment]: <> (MSSedusch också någon typ av automation om distributionen är inte möjligt med hello Azure-portalen. Det går inte att åtgärder som skript distribution av flera virtuella datorer via hello Azure-portalen.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Hantering via Microsoft Azure PowerShell-cmdlets
Windows PowerShell är ett kraftfullt och utökningsbart ramverk som har varit brett bland kunder distribuera större mängder system i Azure. Efter hello installation av PowerShell-cmdlet på en stationär dator, bärbar dator eller dedikerade management station, kan hello PowerShell-cmdlets köras via fjärranslutning.

Hej processen tooenable en lokal desktop dator för hello användning av Azure PowerShell-cmdlets och hur de för hello med tooconfigure hello Azure abonnemang som beskrivs i [i den här artikeln][powershell-install-configure].

Mer detaljerade anvisningar om hur tooinstall, uppdatera och konfigurera hello Azure PowerShell-cmdlets finns i [kapitlet hello Deployment Guide][deployment-guide-4.1].

Customer experience har hittills att PowerShell (PS) är visserligen hello mer kraftfulla verktyg toodeploy virtuella datorer och toocreate anpassade steg i hello distribution av virtuella datorer. Alla hello kunder SAP-instanser som körs i Azure med PS-cmdlets toosupplement hanteringsuppgifter de i hello Azure-portalen eller använder även PS-cmdlets uteslutande toomanage distributioner i Azure. Eftersom hello Azure-specifika cmdletar resursen hello samma namngivningskonvention som hello mer än 2000 Windows-relaterade cmdlets, är det en enkel uppgift för Windows-administratörer tooleverage dessa cmdlets.

Finns det här exemplet: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd TODO Beskriver nya CLI-kommandot när testas)
Distribution av hello Azure övervakning tillägget för SAP (finns i kapitlet [Azure övervakning lösning för SAP] [ planning-guide-9.1] i det här dokumentet) är bara möjligt via PowerShell eller CLI. Därför är obligatorisk tooset upp och konfigurera PowerShell eller CLI när du distribuerar eller administrera en SAP NetWeaver system i Azure.  

Som Azure tillhandahåller fler funktioner, kommer nya PS-cmdlets toobe lagts till som kräver en uppdatering av hello-cmdlets. Därför det gör meningsfullt toocheck hello Azure hämta platsen minst en gång hello månad <https://azure.microsoft.com/downloads/> för en ny version av hello-cmdlets. hello nya versionen installeras ovanpå hello äldre version.

En allmän lista över Azure-relaterade PowerShell kommandon finns här: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Hantering via Microsoft Azure CLI-kommandona
För kunder som använder Linux och vill toomanage Azure kanske resurser Powershell inte är ett alternativ. Microsoft erbjuder Azure CLI som ett alternativ.
hello Azure CLI innehåller en uppsättning med öppen källkod, plattformsoberoende kommandon för att arbeta med hello Azure-plattformen. hello Azure CLI tillhandahåller mycket hello samma funktioner som finns i hello Azure-portalen.

Information om installation, konfiguration och hur toouse CLI-kommandon tooaccomplish finns Azure uppgifter

* [Installera hello Azure CLI][xplat-cli]
* [Distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och hello Azure CLI] [.. /.. / linux/create-ssh-secured-vm-from-template.md]
* [Använd hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager][xplat-cli-azure-resource-manager]

Läs också kapitlet [Azure CLI för virtuella Linux-datorer] [ deployment-guide-4.5.2] i hello [Deployment Guide] [ planning-guide] på hur toouse Azure CLI toodeploy hello Azure-övervakning Tillägget för SAP.

## <a name="different-ways-toodeploy-vms-for-sap-in-azure"></a>Olika sätt toodeploy virtuella datorer för SAP i Azure
I det här kapitlet Läs hello olika sätt toodeploy en virtuell dator i Azure. Förberedelse av ytterligare procedurer, samt hantering av virtuella hårddiskar och virtuella datorer i Azure beskrivs i detta kapitel.

### <a name="deployment-of-vms-for-sap"></a>Distribution av virtuella datorer för SAP
Microsoft Azure tillhandahåller flera olika sätt toodeploy virtuella datorer och associerade diskar. Därför är det mycket viktigt toounderstand hello skillnader eftersom förberedelser hello virtuella datorer kan varierar beroende på hello distributionsmetoden. I allmänhet ta vi en titt på hello följande scenarier:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Flytta en virtuell dator från lokal tooAzure med en icke-generaliserad disk
Du planerar toomove ett särskilt SAP-system från lokala tooAzure. Detta kan göras genom att överföra hello virtuell Hårddisk, vilket innehåller hello OS, hello SAP binärfiler och DBMS binärfiler plus hello virtuella hårddiskar med hello data och loggfilen filer med hello DBMS tooAzure. Däremot för[scenario #2 nedan][planning-guide-5.1.2], du behåller hello värdnamn, SAP-SID och SAP användarkonton i hello Azure VM som de har konfigurerats i hello lokala miljö. Därför är att generalisera hello bilden inte nödvändigt. Se kapitlen [förberedelse för att flytta en virtuell dator från lokal tooAzure med en icke-generaliserad disk] [ planning-guide-5.2.1] i det här dokumentet för lokala förberedelsesteg och överföra ej generaliserad tooAzure för virtuella datorer eller virtuella hårddiskar. Läs kapitel [Scenario 3: flytta en virtuell dator från lokalt med hjälp av en ej generaliserad Azure VHD med SAP] [ deployment-guide-3.4] i hello [Deployment Guide] [ deployment-guide] detaljerade anvisningar för att distribuera en sådan avbildning i Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Distribuera en virtuell dator med en kundspecifika avbildning
På grund av toospecific korrigering kraven i din OS eller DBMS-version, kan hello tillhandahålls bilder i hello Azure Marketplace inte passar dina behov. Därför kanske du måste toocreate en VM som använder en egen ”privat” OS/DBMS VM-avbildning som kan distribueras efteråt flera gånger. tooprepare en ”privat” avbildning för duplicering hello följande objekt har toobe anses vara:

- - -
> ![Windows][Logo_Windows] Windows
>
> Se mer information här: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> hello Windows-inställningar (t.ex. Windows-SID och värdnamn) måste vara tas ut/generaliserad hello på plats Virtuella datorn via hello sysprep-kommandot.
>
>
> ![Linux][Logo_Linux] Linux
>
> Följ hello stegen som beskrivs i följande artiklar innehåller [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], eller [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle], tooprepare en VHD-toobe upp tooAzure.
>
>

- - -
Om du redan har installerat SAP innehållet i den lokala virtuella datorn (särskilt för nivå 2-system), kan du anpassa hello SAP systeminställningar när hello distribution av hello Azure VM via hello förekomst proceduren som stöds av hello SAP tillhandahållande av programvara Manager (SAP-kommentar [1619720]). Se kapitlen [förberedelse för att distribuera en virtuell dator med en avbildning av kundspecifika SAP] [ planning-guide-5.2.2] och [ladda upp en VHD från lokala tooAzure] [ planning-guide-5.3.2]i det här dokumentet för lokala förberedelser och överföring av en generaliserad virtuell tooAzure. Läs kapitel [Scenario 2: distribuera en virtuell dator med en anpassad avbildning för SAP] [ deployment-guide-3.3] i hello [Deployment Guide] [ deployment-guide] detaljerade anvisningar för att distribuera sådana en avbildning i Azure.

#### <a name="deploying-a-vm-out-of-hello-azure-marketplace"></a>Distribuera en virtuell dator utanför hello Azure Marketplace
Du skulle vilja toouse en Microsoft eller tredje parts tillhandahålls VM-avbildning från hello Azure Marketplace toodeploy den virtuella datorn. När du har distribuerat den virtuella datorn i Azure måste du följa hello samma riktlinjer och verktyg tooinstall hello SAP-program och/eller DBMS inuti den virtuella datorn som du gör i en lokal miljö. Mer detaljerad distributionsbeskrivning av finns kapitel [Scenario 1: distribuera en virtuell dator utanför hello Azure Marketplace för SAP] [ deployment-guide-3.2] i hello [Deployment Guide] [ deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Förbereda virtuella datorer med SAP för Azure
Innan du laddar upp virtuella datorer till Azure måste du toomake att uppfylla vissa krav hello virtuella datorer och virtuella hårddiskar. Det finns mindre skillnader beroende hello distributionsmetod som används.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Förberedelser för att flytta en virtuell dator från lokal tooAzure med en icke-generaliserad disk
En vanlig distributionsmetod är toomove en befintlig virtuell dator som kör ett SAP-system från lokala tooAzure. Att VM hello SAP system i hello VM bara ska köras i Azure med hjälp av hello samma värdnamn och mycket sannolikt hello samma SAP-SID. I det här fallet bör hello gäst OS VM inte vara generaliserad för flera distributioner. Om hello lokalt nätverk har utökats till Azure (se kapitel [mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med hello behovet av att fullständigt integrerat i hello lokalt nätverk] [ planning-guide-2.2] i det här dokumentet), sedan även hello samma domänkonton kan användas inom hello VM som de som användes innan lokalt.

Kraven när du förbereder din egen Azure VM-disken är:

* Ursprungligen kan hello VHD som innehåller hello operativsystem ha endast en maximal storlek på 127GB. Den här begränsningen har eliminerats hello slutet av mars 2015. Nu kan hello virtuell Hårddisk som innehåller operativsystemet för hello vara upp too1TB i storlek som VHD finns också i Azure Storage.
* Den måste toobe i hello fast VHD-format. Dynamiska virtuella hårddiskar eller virtuella hårddiskar i VHDx-format stöds inte ännu i Azure. Dynamiska virtuella hårddiskar är konverterade toostatic virtuella hårddiskar när du överför hello VHD med PowerShell-kommandon eller CLI
* Virtuella hårddiskar som är monterade toohello VM och ska monteras igen i Azure toohello VM behöver toobe i ett fast VHD-format samt. Läs [(Linux) för den här artikeln](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) och [i den här artikeln (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) för storleksgränser för datadiskar. Dynamiska virtuella hårddiskar är konverterade toostatic virtuella hårddiskar när du överför hello VHD med PowerShell-kommandon eller CLI
* Lägg till en annan lokalt konto med administratörsbehörighet som kan användas av Microsoft-supporten eller som kan tilldelas för tjänster och program toorun i tills hello VM distribueras och mer lämpliga användare kan användas.
* För hello fall för att använda en molnbaserad distributionsscenariot (finns i kapitlet [endast molnbaserad - distributioner på virtuella datorer till Azure utan beroenden på hello lokalt nätverk för kundens] [ planning-guide-2.1] av denna Dokumentera) i kombination med den här distributionsmetoden, domän konton inte kanske fungerar när hello Azure-disken har distribuerats i Azure. Detta gäller särskilt för konton som används toorun tjänster som hello DBMS eller SAP-program. Därför behöver tooreplace sådana domänkonton med VM lokala konton och ta bort hello lokalt domänkonton i hello VM. Behålla lokalt domänanvändare i hello VM-avbildning är inte ett problem när hello VM distribueras i hello mellan lokala scenario som beskrivs i kapitlet [mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med hello behovet av att fullständigt integrerat i hello lokalt nätverk] [ planning-guide-2.2] i det här dokumentet.
* Om domänkonton användes som DBMS-inloggningar eller användare när du kör hello system lokalt och dessa virtuella datorer som är avsedda toobe som distribuerats i endast molnbaserad scenarier, måste hello domänanvändare toobe tas bort. Du måste toomake till som lokal administratör för hello plus en annan VM lokal användare läggs till som en inloggning i hello DBMS som administratörer.
* Lägg till andra lokala konton som de kan behövas för hello specifika distributionsscenariot.

- - -
> ![Windows][Logo_Windows] Windows
>
> I det här scenariot inga generalisering (sysprep) av hello VM är nödvändiga tooupload och distribuera hello VM på Azure.
> Kontrollera att enheten D:\ inte används.
> Ange disken automount för anslutna diskar som beskrivs i kapitel [inställningen automount för anslutna diskar] [ planning-guide-5.5.3] i det här dokumentet.
>
> ![Linux][Logo_Linux] Linux
>
> I det här scenariot inga generalisering (waagent-deprovision) hello VM är nödvändiga tooupload och distribuera hello VM på Azure.
> Kontrollera att/mnt/resurs inte används och att alla diskar är monterade via uuid. Se till att hello startprogrammet post också avspeglar hello uuid-baserade montering för hello OS-disken.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Förberedelser för att distribuera en virtuell dator med en avbildning av kundspecifika för SAP
VHD-filer som innehåller en generaliserad OS lagras i behållare på Azure Storage-konton eller som hanteras diskavbildningar. Du kan distribuera en ny virtuell dator från en sådan avbildning genom att referera hello VHD- eller hanteras Disk image som källa i din distribution mallfilerna som beskrivs i kapitlet [Scenario 2: distribuera en virtuell dator med en anpassad avbildning för SAP] [ deployment-guide-3.3]av hello [Deployment Guide][deployment-guide].

Kraven när du förbereder din egen Azure VM-avbildning är:

* Ursprungligen kan hello VHD som innehåller hello operativsystem ha endast en maximal storlek på 127GB. Den här begränsningen har eliminerats hello slutet av mars 2015. Nu kan hello virtuell Hårddisk som innehåller operativsystemet för hello vara upp too1TB i storlek som VHD finns också i Azure Storage.
* Den måste toobe i hello fast VHD-format. Dynamiska virtuella hårddiskar eller virtuella hårddiskar i VHDx-format stöds inte ännu i Azure. Dynamiska virtuella hårddiskar är konverterade toostatic virtuella hårddiskar när du överför hello VHD med PowerShell-kommandon eller CLI
* Virtuella hårddiskar som är monterade toohello VM och ska monteras igen i Azure toohello VM behöver toobe i ett fast VHD-format samt. Läs [(Linux) för den här artikeln](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) och [i den här artikeln (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) för storleksgränser för datadiskar. Dynamiska virtuella hårddiskar är konverterade toostatic virtuella hårddiskar när du överför hello VHD med PowerShell-kommandon eller CLI
* Eftersom alla hello domänanvändare som är registrerad som användare i hello VM inte finns i ett scenario med endast molnbaserad (finns i kapitlet [endast molnbaserad - distributioner på virtuella datorer till Azure utan beroenden på hello lokalt nätverk för kundens] [ planning-guide-2.1] i det här dokumentet), tjänster som använder dessa konton inte kanske fungerar när hello avbildningen har distribuerats i Azure-domän. Detta gäller särskilt för konton som används toorun tjänster som DBMS eller SAP-program. Därför behöver tooreplace sådana domänkonton med VM lokala konton och ta bort hello lokalt domänkonton i hello VM. Behålla lokalt domänanvändare i hello VM-avbildning kanske inte är ett problem när hello VM distribueras i hello mellan lokala scenario som beskrivs i kapitlet [mellan lokala - distribution av en eller flera SAP virtuella datorer i Azure med hello krav är integrerat i hello lokalt nätverk] [ planning-guide-2.2] i det här dokumentet.
* Lägg till en annan lokalt konto med administratörsbehörighet som kan användas av Microsoft-supporten i problemet undersökningar eller som kan tilldelas för tjänster och program toorun i tills hello VM distribueras och mer lämpliga användare kan användas.
* I endast molnbaserad distributioner och domänkonton användes som DBMS-inloggningar eller användare när du kör hello system lokala ska hello domänanvändare tas bort. Du måste toomake till att hello lokal administratör plus en annan VM lokal användare läggs till som en inloggning/användare av hello DBMS som administratörer.
* Lägg till andra lokala konton som de kan behövas för hello specifika distributionsscenariot.
* Om hello bilden innehåller en installation av SAP NetWeaver och namnbyte av hello värdnamn från hello ursprungliga namn hello punkt i hello Azure-distribution är troligt är det rekommenderade toocopy hello senaste versionerna av hello SAP programvara etablering Manager DVD i hello mall. Detta gör att du tooeasily Använd hello SAP förutsatt att byta funktioner tooadapt hello ändras värdnamn och/eller ändra hello SID för hello SAP system inom hello distribueras VM-avbildning som en ny kopia har startats.

- - -
> ![Windows][Logo_Windows] Windows
>
> Se till att enheten inte är D:\ användas Set disk automount för anslutna diskar som beskrivs i kapitlet [inställningen automount för anslutna diskar] [ planning-guide-5.5.3] i det här dokumentet.
>
> ![Linux][Logo_Linux] Linux
>
> Kontrollera att/mnt/resurs inte används och att alla diskar är monterade via uuid. Kontrollera hello startprogrammet också redigera hello uuid-baserade montering för hello OS-disken.
>
>

- - -
* SAP GUI (för administrativa och konfigurera ändamål) kan förinstalleras i denna mall.
* Annan programvara som behövs toorun hello virtuella datorer har i mellan lokala scenarier kan installeras så länge som det här programmet kan arbeta med hello Byt namn på av hello VM.

Om hello VM förbereds tillräckligt toobe generisk och kan slutligen oberoende av konton/användare inte tillgängligt i hello Azure distributionsscenariot som mål, hello senaste förberedelsen för att generalisera sådana en avbildning utförs.

##### <a name="generalizing-a-vm"></a>Att generalisera en virtuell dator
- - -
> ![Windows][Logo_Windows] Windows
>
> hello sista steget är toolog i tooa VM med ett administratörskonto. Öppna en Windows-kommandofönster som ”administratör”. Gå too%windir%\windows\system32\sysprep och köra sysprep.exe.
> Ett litet fönster visas. Det är viktigt toocheck hello 'Generalize' alternativet (hello standard är inte markerad) och ändra hello Avslutningsalternativ från standard ”starta om' too'Shutdown'. Den här proceduren förutsätter att hello sysprep-processen körs lokalt i hello Gästoperativsystem på en virtuell dator.
> Tooperform hello proceduren med en virtuell dator som redan körs i Azure, så du hello stegen som beskrivs i [i den här artikeln](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Hur toocapture en Linux virtuella toouse som en Resource Manager-mall][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-tooazure"></a>Överföring av virtuella datorer och virtuella hårddiskar mellan lokala tooAzure
Eftersom det inte går att överföra VM-avbildningar och diskar tooAzure via hello Azure-portalen, behöver du toouse Azure PowerShell-cmdlets eller CLI. En annan möjlighet är hello använda hello verktyget 'AzCopy'. hello-verktyget kan kopiera virtuella hårddiskar mellan lokala och Azure (i båda riktningarna). Det kan också kopiera virtuella hårddiskar mellan Azure-regioner. Kontakta [denna dokumentation] [ storage-use-azcopy] om hämtning och användning av AzCopy.

Ett tredje alternativ skulle vara toouse indatavärdena GUI-verktyg för olika från tredje part. Dock se till att verktygen stöder Azure Sidblobar. För våra ändamål behöver vi toouse Azure Sidblob butiken (hello skillnaderna beskrivs här: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). Hello verktyg som tillhandahålls av Azure är också mycket effektivt för komprimering av hello virtuella datorer och virtuella hårddiskar som behöver toobe upp. Detta är viktigt eftersom den här effektiviteten i komprimeringen minskar hello tid (som varierar ändå hello överför länken toohello internet från hello lokal anläggning och hello Azure-distribution region mål). Det är en verkligt antagandet att ladda upp en virtuell dator eller en VHD från Europeiska plats toohello USA-baserade Azure data Datacenter tar längre tid än överföring hello samma virtuella datorer/virtuella hårddiskar toohello Europeiska Azure-datacenter.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Ladda upp en VHD från lokala tooAzure
tooupload en befintlig virtuell dator eller en VHD från hello lokalt nätverk sådana en virtuell dator eller virtuell Hårddisk måste toomeet hello krav enligt kapitel [förberedelse för att flytta en virtuell dator från lokal tooAzure med en icke-generaliserad disk] [ planning-guide-5.2.1] i det här dokumentet.

Sådana en virtuell dator behöver inte toobe generaliserad och kan överföras i hello tillstånd och formen som den har efter avstängning på hello lokalt på klientsidan. hello sak samma gäller för ytterligare virtuella hårddiskar som inte innehåller något operativsystem.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Överför en virtuell Hårddisk och gör det Azure-disken
I det här fallet vi tooupload en virtuell Hårddisk, med eller utan ett operativsystem, och montera den tooa VM som en datadisk eller använda den som OS-disken. Detta är en process med flera steg

**PowerShell**

* Logga in tooyour prenumerationen med *Login-AzureRmAccount*
* Ange hello prenumeration för din kontext med *Set-AzureRmContext* och parametern prenumerations-ID eller SubscriptionName - Se <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Överför hello VHD med *Lägg till AzureRmVhd* tooan Azure Storage-konto - finns <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Valfritt) Skapa en Disk som hanteras av hello VHD med *ny AzureRmDisk* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk>
* Ange hello OS-disken på en ny VM config toohello VHD eller hanteras med *Set AzureRmVMOSDisk* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
* Skapa en ny virtuell dator från hello konfigurationen med *ny AzureRmVM* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>
* Lägg till en data disk tooa ny virtuell dator med *Lägg till AzureRmVMDataDisk* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk>

**Azure CLI 2.0**

* Logga in tooyour prenumerationen med *az inloggning*
* Välj din prenumeration med *az inställt--prenumeration`<subscription name or id`>*
* Överför hello VHD med *az storage blob överför* -finns [Using hello Azure CLI med Azure Storage][storage-azure-cli]
* (Valfritt) Skapa en Disk som hanteras av hello VHD med *az disk skapa* -finns https://docs.microsoft.com/cli/azure/disk#create
* Skapa en ny virtuell dator att ange hello överförs VHD- eller hanterade disken som OS-disken med *az vm skapa* och parametern *--bifoga-os-disk*
* Lägg till en data disk tooa ny virtuell dator med *az vm disk bifoga* och parametern *--nya*

**Mall**

* Överför hello VHD med Powershell eller Azure CLI
* (Valfritt) Skapa en Disk som hanteras av hello VHD med Powershell, Azure CLI eller hello Azure-portalen
* Distribuera hello VM med en JSON-mall som refererar till hello VHD enligt [exempel JSON mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json) eller med hjälp av hanterade diskar enligt [exempel JSON mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-disk-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Distribution av en VM-avbildning
tooupload en befintlig virtuell dator eller en VHD från hello lokalt nätverk i ordning toouse som en Azure VM-avbildning sådana en virtuell dator eller virtuell Hårddisk måste toomeet hello krav som anges i kapitel [förberedelse för att distribuera en virtuell dator med en avbildning av kundspecifika SAP] [ planning-guide-5.2.2] i det här dokumentet.

* Använd *sysprep* i Windows eller *waagent-deprovision* på Linux toogeneralize dina VM - finns [Teknisk referens för Sysprep](https://technet.microsoft.com/library/cc766049.aspx) för Windows eller [hur toocapture en Linux virtuella toouse som en Resource Manager-mall] [ capture-image-linux-step-2-create-vm-image] för Linux
* Logga in tooyour prenumerationen med *Login-AzureRmAccount*
* Ange hello prenumeration för din kontext med *Set-AzureRmContext* och parametern prenumerations-ID eller SubscriptionName - Se <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Överför hello VHD med *Lägg till AzureRmVhd* tooan Azure Storage-konto - finns <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Valfritt) Skapa en hanterad diskavbildning från hello VHD med *ny AzureRmImage* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage>
* Ange hello OS-disk på en ny VM config toothe
  * Virtuell Hårddisk med *Set-AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
  * Hanterade diskavbildning *Set AzureRmVMSourceImage* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage>
* Skapa en ny virtuell dator från hello konfigurationen med *ny AzureRmVM* -finns <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>

**Azure CLI 2.0**

* Använd *sysprep* i Windows eller *waagent-deprovision* på Linux toogeneralize dina VM - finns [Teknisk referens för Sysprep](https://technet.microsoft.com/library/cc766049.aspx) för Windows eller [hur toocapture en Linux virtuella toouse som en Resource Manager-mall] [ capture-image-linux-step-2-create-vm-image] för Linux
* Logga in tooyour prenumerationen med *az inloggning*
* Välj din prenumeration med *az inställt--prenumeration`<subscription name or id`>*
* Överför hello VHD med *az storage blob överför* -finns [Using hello Azure CLI med Azure Storage][storage-azure-cli]
* (Valfritt) Skapa en hanterad diskavbildning från hello VHD med *az bild skapa* -finns https://docs.microsoft.com/cli/azure/image#create
* Skapa en ny virtuell dator att ange hello överförs VHD- eller hanteras diskavbildning som OS-disken med *az vm skapa* och parametern *--bild*

**Mall**

* Använd *sysprep* i Windows eller *waagent-deprovision* på Linux toogeneralize dina VM - finns [Teknisk referens för Sysprep](https://technet.microsoft.com/library/cc766049.aspx) för Windows eller [hur toocapture en Linux virtuella toouse som en Resource Manager-mall] [ capture-image-linux-step-2-create-vm-image] för Linux
* Överför hello VHD med Powershell eller Azure CLI
* (Valfritt) Skapa en hanterad diskavbildning från hello VHD med Powershell, Azure CLI eller hello Azure-portalen
* Distribuera hello VM med en JSON-mall som refererar till avbildningen hello VHD enligt [exempel JSON mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-image/azuredeploy.json) eller med hjälp av hello hanteras diskavbildning enligt [exempel JSON mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-tooon-premises"></a>Hämta VHD eller hanterade diskar tooon lokala
Azure-infrastrukturen som en tjänst är inte en enkelriktad gata att endast kan tooupload virtuella hårddiskar och SAP-system. Du kan flytta SAP-system från Azure tillbaka till lokala hälsningsmeddelande samt.

Hämta hello VHD eller hanterade diskar kan inte vara aktiva under hello tiden för hello. Även när du hämtar diskar som är monterade tooVMs hello VM behov toobe stängs av och frigöra. Om du bara vill toodownload hello databasen innehåll som sedan ska använda tooset in ett nytt system lokalt och om det är acceptabelt att under hello tiden för hello hämtar och hello installationen av hello nya system som hello system i Azure kan fortfarande tas i bruk , du kan undvika långa driftstopp genom att utföra en säkerhetskopiering av komprimerade databasen till en disk och bara hämta disken i stället för hämtar hello OS grundläggande VM.

#### <a name="powershell"></a>PowerShell

  * Hämta hanterade diskar  
  Du måste först tooget åtkomst toohello underliggande blob för hello hanterade disken. Sedan kan du kopiera hello underliggande blob tooa nya storage-konto och hämta hello blob storage-konto.

  ```powershell
  $access = Grant-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzureRmStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzureStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzureStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy toofinish
  Get-AzureStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzureRmVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download toofinish
  Revoke-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

  * Hämta en virtuell Hårddisk  
  När hello SAP-systemet har stoppats och hello VM är avstängd, du kan använda hello PowerShell-cmdleten spara AzureRmVhd hello lokalt VHD måldiskarna toodownload hello tillbaka toohello lokalt världen. I ordning toodo att du behöver hello VHD som du hittar i hello-avsnittet lagring' hello URL för hello behöver Azure-portalen (behovet toonavigate toohello Storage-konto och hello lagringsbehållaren där hello VHD skapades) och du tooknow där hello VHD ska kopieras till.

  Du kan sedan använda hello kommandot genom att helt enkelt definiera hello parametern SourceUri som hello URL hello VHD toodownload och hello LocalFilePath som hello fysiska platsen för hello VHD (inklusive filnamnet). hello-kommandot kan se ut så:

  ```powerhell
  Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Mer information om hello spara AzureRmVhd cmdlet Kontrollera här <https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvhd>.

#### <a name="cli-20"></a>CLI 2.0
  * Hämta hanterade diskar  
  Du måste först tooget åtkomst toohello underliggande blob för hello hanterade disken. Sedan kan du kopiera hello underliggande blob tooa nya storage-konto och hämta hello blob storage-konto.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

  * Hämta en virtuell Hårddisk   
  När hello SAP-systemet har stoppats och hello VM är avstängd, kan du använda hello Azure CLI kommandot _azure storage blob download_ på hello lokala målet toodownload hello VHD diskar tillbaka toohello lokalt världen. I ordning toodo att du behöver hello namn och hello VHD som du hittar i hello-avsnittet lagring' hello behållare för hello Azure-portalen (behovet toonavigate toohello Storage-konto och hello lagringsbehållaren där hello VHD skapades) och du behöver tooknow där hello virtuella Hårddisken ska kopieras till.

  Du kan sedan använda hello kommandot genom att helt enkelt definiera hello parametrar blob och behållare för hello VHD toodownload och hello mål som hello fysiska platsen för hello VHD (inklusive filnamnet). hello-kommandot kan se ut så:

  ```
  az storage blob download --name <name of hello VHD toodownload> --container-name <container of hello VHD toodownload> --account-name <storage account name of hello VHD toodownload> --account-key <storage account key> --file <destination of hello VHD toodownload>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>Överföring av virtuella datorer och diskar i Azure
#### <a name="copying-sap-systems-within-azure"></a>Kopiera SAP-system i Azure
Ett SAP-system eller även en dedikerad DBMS server som stöder ett lager för SAP-program kommer sannolikt består av flera diskar som innehåller antingen hello OS med hello binärfiler eller hello data och loggfiler för hello SAP-databasen. Varken hello Azure funktioner för att kopiera diskar eller hello Azure funktioner för att spara diskar tooa lokal disk har en synkroniseringsmekanism som flera diskar skulle ögonblicksbild synkront. Därför hello hello kopieras eller spara diskar, även om de är monterade samma virtuella dator mot hello skulle vara olika. Detta innebär att hello konkreta gäller att ha olika data och logfile(s) i hello olika diskar, hello databasen i slutet av hello skulle vara inkonsekvent.

**Slutsats: I ordning toocopy eller spara diskar som är en del av en SAP konfiguration du behöver toostop hello SAP-systemet och måste också tooshut ned hello distribueras VM. Sedan kan du kopiera eller ladda ned hello uppsättning diskar tooeither skapar du en kopia av hello SAP system i Azure eller lokalt.**

Datadiskar kan lagras som VHD-filer i ett Azure Storage-konto och direktansluten tooa virtuell dator eller användas som en bild. I det här fallet är hello VHD kopierade tooanother plats innan den bifogade toohello virtuell dator. hello fullständiga namn hello VHD-filen i Azure måste vara unikt i Azure. Som nämnts tidigare redan är hello typ av en tredelat namn som ser ut som:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Datadiskar kan också vara hanterade diskar. I det här fallet är hello hanterade disken används toocreate nya hanterade disken innan du bifogade toohello virtuell dator. hello namnet på hello hanterad Disk måste vara unika inom en resursgrupp.

##### <a name="powershell"></a>PowerShell
Du kan använda Azure PowerShell-cmdlets toocopy en virtuell Hårddisk enligt [i den här artikeln][storage-powershell-guide-full-copy-vhd]. toocreate en ny Disk i hanterad Använd ny AzureRmDiskConfig och ny AzureRmDisk som visas i följande exempel hello.

```powershell
$config = New-AzureRmDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzureRmDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="cli-20"></a>CLI 2.0
Du kan använda Azure CLI toocopy en virtuell Hårddisk enligt [i den här artikeln][storage-azure-cli-copy-blobs]. toocreate en ny hanterad Disk, Använd *az disk skapa* som visas i följande exempel hello.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Azure Storage-verktyg
* <http://storageexplorer.com/>

Professional-versionerna av Azure lagringsutforskare finns här:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

hello kopia av en VHD: n i ett lagringskonto är en process som tar endast några sekunder (liknande tooSAN maskinvara skapar ögonblicksbilder med lazy kopia och kopiera vid skrivning till). När du har en kopia av hello VHD-filen kan du koppla den tooa virtuell dator eller Använd den som en bild tooattach kopierar hello VHD toovirtual datorer.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path toovhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a managed disk tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of hello vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path tooimage vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzureRmVM

# attach a copy of hello managed disk tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzureRmDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzureRmDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzureRmVM
```
##### <a name="cli-20"></a>CLI 2.0
```

# attach a vhd tooa vm
az vm unmanaged-disk attach --resource-group <resource group name> --vm-name <vm name> --vhd-uri <path toovhd>

# attach a managed disk tooa vm
az vm disk attach --resource-group <resource group name> --vm-name <vm name> --disk <managed disk id>

# attach a copy of hello vhd tooa vm
# this scenario is currently not possible with Azure CLI. A workaround is toomanually copy hello vhd toohello destination.

# attach a copy of a managed disk tooa vm
az disk create --name <new disk name> --resource-group <resource group name> --location <location of target virtual machine> --source <source managed disk id>
az vm disk attach --disk <new disk name or managed disk id> --resource-group <resource group name> --vm-name <vm name> --caching <caching option> --lun <lun, for example 0>
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Kopiera diskar mellan Azure Storage-konton
Den här uppgiften kan inte utföras på hello Azure-portalen. Du kan använda Azure PowerShell-cmdlets, Azure CLI eller en webbläsare från tredje part lagring. Hej PowerShell-cmdlets eller CLI-kommandon kan skapa och hantera blobbar, bland annat hello möjlighet tooasynchronously kopiera BLOB mellan Lagringskonton och mellan regioner inom hello Azure-prenumeration.

##### <a name="powershell"></a>PowerShell
Du kan också kopiera virtuella hårddiskar mellan prenumerationer. Mer information finns [i den här artikeln][storage-powershell-guide-full-copy-vhd].

hello grundläggande flödet av hello PS cmdlet logik ser ut så här:

* Skapa en kontexten för lagringskontot för hello **källa** lagringskonto med *ny AzureStorageContext* -finns <https://msdn.microsoft.com/library/dn806380.aspx>
* Skapa en kontexten för lagringskontot för hello **mål** lagringskonto med *ny AzureStorageContext* -finns <https://msdn.microsoft.com/library/dn806380.aspx>
* Starta hello kopian med

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Kontrollera hello hello kopia i en slinga med status

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Koppla hello nya VHD tooa virtuella enligt beskrivningen ovan.

Exemplen finns [i den här artikeln][storage-powershell-guide-full-copy-vhd].

##### <a name="cli-20"></a>CLI 2.0
* Starta hello kopian med

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Kontrollera status för hello om hello kopia i en slinga med

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Koppla hello nya VHD tooa virtuella enligt beskrivningen ovan.

Exemplen finns [i den här artikeln][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Disk-hantering
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Virtuella datorn/disken strukturen för SAP-distributioner
Helst hello hantering av hello struktur för en virtuell dator och associerade hello diskar bör vara mycket enkelt. Kunder utvecklat många sätt att strukturera en serverinstallation lokala installationer.

* En grundläggande disk som innehåller hello OS och alla hello binärfilerna för hello DBMS och/eller SAP. Sedan mars 2015, kan den här disken in too1TB i storlek i stället för tidigare begränsningar som begränsade den vara too127GB.
* En eller flera diskar som innehåller hello DBMS loggfilen för hello SAP-databasen och hello loggfil med hello DBMS temporär lagringsplats (om hello DBMS stöder detta). Om hello-loggen IOPS databaskrav är högt, måste toostripe flera diskar i ordning tooreach hello IOPS volymen som krävs.
* Ett antal diskar som innehåller en eller två databasfiler för hello SAP-databasen och hello DBMS temporära filer samt (om hello DBMS stöder detta).

![För referenskonfiguration av Azure IaaS-VM för SAP][planning-guide-figure-1300]

[comment]: <> (MShermannd TODO beskrivs Linux struktur)

- - -
> ![Windows][Logo_Windows] Windows
>
> Med många kunder såg vi konfigurationer, till exempel SAP och DBMS binärfiler har inte installerats på hello c:\ enhet där hello OS installerades. Det finns olika orsaker, men när vi tillbaka toohello rot, var vanligtvis att hello enheter var små och OS uppgraderingar behövs ytterligare utrymme 10 – 15 år sedan. Båda villkoren gäller inte för ofta längre dessa dagar. Idag mappas hello c:\ enhet diskar med stora volymer eller på virtuella datorer. I ordning tookeep distributioner enkel i sin struktur, är det rekommenderade toofollow följande distribution mönster för SAP NetWeaver system i Azure
>
> hello Windows operativsystem växlingsfilens storlek bör vara på hello D: enhet (icke-beständig disk)
>
> ![Linux][Logo_Linux] Linux
>
> Placera hello Linux växlingsfil under /mnt/mnt/resurs på Linux som beskrivs i [i den här artikeln][virtual-machines-linux-agent-user-guide]. hello växlingsfil kan konfigureras i hello Linux-agenten /etc/waagent.conf hello konfigurationsfil. Lägga till eller ändra hello följande inställningar:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

tooactivate hello ändringar behöver du toorestart hello Linux-agenten med

```
sudo service waagent restart
```

Läs SAP-kommentar [1597355] för mer information om hello rekommenderar filstorlek i byte

- - -
hello antalet diskar som används för hello DBMS-datafiler och hello typ av Azure Storage finns på diskarna ska fastställas av hello IOPS krav och hello fördröjning krävs. Exakt kvoter beskrivs i [(Linux) för den här artikeln] [ virtual-machines-sizes-linux] och [i den här artikeln (Windows)][virtual-machines-sizes-windows].

Erfarenhet av SAP distributioner över hello senaste 2 åren vilka undervisning förekommer oss vissa erfarenheter som kan sammanfattas som:

* IOPS trafik toodifferent datafiler är inte alltid hello samma eftersom befintliga kunden system kan ha olika storlek data filer som representerar sina SAP-databaserna. Därför aktiverad ut toobe bättre med en RAID-konfiguration över flera diskar tooplace hello datafiler LUN högg utanför de. Det fanns situationer, särskilt med Azure standardlagring där en IOPS frekvens uppnått hello kvoten av en enskild disk mot hello DBMS transaktionsloggen. I sådana situationer hello Premium-lagring bör du använda eller också sammanställa flera standardlagring RAID-diskar med en programuppdatering.

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
* Premium-lagring visas betydande bättre prestanda, särskilt för kritiska transaction log skrivningar. SAP-scenarier som är förväntade toodeliver produktionsliknande prestanda, rekommenderas det starkt toouse VM-serien som kan använda Azure Premium-lagring.

Tänk på att hello-disk som innehåller hello OS och som vi rekommenderar hello binärfilerna för SAP och hello databasen (grundläggande VM), är inte längre begränsade too127GB. Nu kan ha upp too1TB i storlek. Detta bör vara tillräckligt med utrymme tookeep alla hello inklusive nödvändig fil, till exempel, SAP-loggarna för batch-jobbet.

För fler förslag och mer specifikt för DBMS virtuella datorer kan du kontakta hello [DBMS Deployment Guide][dbms-guide]

#### <a name="disk-handling"></a>Disk-hantering
I de flesta fall måste du toocreate ytterligare diskar i ordning toodeploy hello SAP-databasen i hello VM. Hittills har diskuterats hello överväganden för antalet diskar i kapitel [virtuella datorn/disken strukturen för SAP distributioner] [ planning-guide-5.5.1] i det här dokumentet. hello Azure-portalen kan tooattach och frånkoppla diskarna när en grundläggande virtuell dator har distribuerats. hello diskar kan vara kopplad/borttagbar när hello VM är igång och körs samt när den har stoppats. När du ansluter en disk hello Azure-portalen tillhandahåller tooattach en tom disk eller en befintlig disk som vid denna tidpunkt inte är ansluten tooanother VM.

**Obs**: diskar kan bara vara anslutna tooone VM i en given tidpunkt.

![Koppla / frånkoppla diskarna med Azure standardlagring][planning-guide-figure-1400]

Under hello distributionen av en ny virtuell dator kan bestämma du om du vill toouse hanterade diskar eller placera diskarna på Azure Storage-konton. Om du vill toouse Premium-lagring, bör du använda hanterade diskar.

Sedan måste toodecide om du vill toocreate en ny och tom disk eller om du vill att tooselect en befintlig disk som överfördes tidigare och ska vara anslutna toohello VM nu.

**VIKTIGA**: du **inte** vill toouse som värden cachelagring med Azure standardlagring. Hello värden inställningar bör du lämna på hello standard NONE. Med Azure Premium Storage bör du aktivera Läs cachelagring om hello i/o-egenskap är främst läsa som vanliga i/o-trafik mot databasen datafiler. Om databasen transaktionsloggfilen rekommenderas ingen cachelagring.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Hur tooattach en disk i hello Azure-portalen][virtual-machines-linux-attach-disk-portal]
>
> Om diskar har anslutits måste toolog i toohello VM tooopen hello Windows Disk Manager. Om automount inte aktiveras som rekommenderas i kapitel [inställningen automount för anslutna diskar][planning-guide-5.5.3], hello just kopplat volymen behöver toobe tas online och initieras.
>
> ![Linux][Logo_Linux] Linux
>
> Om diskar har anslutits du behöver toolog i toohello VM och initiera hello diskar som beskrivs i [i den här artikeln][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Om hello nya disken är en tom disk, behöver du tooformat hello samt disk. För att formatera, särskilt för DBMS hello data och loggfiler samma rekommendationer för bare metal-distributioner av hello DBMS tillämpas.

Som nämnts i kapitlet [hello Microsoft Azure virtuella konceptet][planning-guide-3.2], ett Azure Storage-konto tillhandahåller inte oändlig resurser i i/o-volym IOPS och data volym. DBMS virtuella datorer som vanligtvis mest påverkas av detta. Om du har några höga i/o-volym VMs toodeploy i ordning toostay inom hello hello Azure Storage-konto volym kan det vara bästa toouse ett separat Lagringskonto för varje virtuell dator. Annars måste toosee hur du balanserar dessa virtuella datorer mellan olika Storage-konton utan att hello gränsen på varje enskild Storage-konto. Mer information i hello beskrivs [DBMS Deployment Guide][dbms-guide]. Du bör också tänka dessa begränsningar på för ren SAP-program virtuella server-datorer eller andra virtuella datorer som eventuellt kan kräva ytterligare virtuella hårddiskar. Dessa begränsningar gäller inte om du använder hanterade disken. Om du planerar toouse Premium-lagring, bör du använda hanterade disken.

Ett annat ämne som är relevant för Storage-konton är om hello virtuella hårddiskar i ett Lagringskonto får georeplikerad. GEO-replikering är aktiverat eller inaktiverat på hello Lagringskonto nivå och inte på hello VM-nivå. Om geo-replikering är aktiverat hello hello virtuella hårddiskar inom hello Storage-konto kan replikeras till en annan Azure-datacenter i samma region. Innan du bestämmer dig för detta, bör du tänka hello följande begränsningar:

Azure Geo-replikering fungerar lokalt på varje virtuell Hårddisk på en virtuell dator och replikerar inte hello IOs i kronologisk ordning över flera virtuella hårddiskar på en virtuell dator. Därför hello VHD som representerar hello grundläggande VM samt eventuella ytterligare virtuella hårddiskar anslutna toohello VM replikeras oberoende av varandra. Det innebär att det finns ingen synkronisering mellan hello ändringar i hello olika virtuella hårddiskar. hello att hello IOs replikeras oberoende av hello ordning i vilken de skrivs innebär att geo-replikering inte är av värdet för databasservrar som har sina databaser fördelade över flera virtuella hårddiskar. I tillägg toohello DBMS finnas det också andra program där processer skriva eller ändra data i olika virtuella hårddiskar och där det är viktigt tookeep hello ordningen på ändringar. Om detta är ett krav, bör inte geo-replikering i Azure aktiveras. Beroende på om du behöver eller vill geo-replikering för en uppsättning virtuella datorer, men inte för en annan uppsättning, du kan redan kategorisera virtuella datorer och deras relaterade virtuella hårddiskar i olika Storage-konton som har geo-replikering aktiverat eller inaktiverat.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Ange automount för anslutna diskar
- - -
> ![Windows][Logo_Windows] Windows
>
> För virtuella datorer som har skapats från egna avbildningar eller diskar, är det nödvändigt toocheck och eventuellt ange hello automount parametern. Den här parametern kan hello VM efter en omstart eller Omdistributionen i Azure toomount hello kopplad/monterade enheter igen automatiskt.
> hello-parameter har angetts för hello bilder som tillhandahålls av Microsoft i hello Azure Marketplace.
>
> I ordning tooset hello automount Kontrollera hello dokumentationen för hello kommandorad körbara diskpart.exe här:
>
> * [Kommandoradsalternativ för DiskPart](https://technet.microsoft.com/library/bb490893.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
>
> hello Windows kommandoradsfönster bör öppnas som administratör.
>
> Om diskar har anslutits måste toolog i toohello VM tooopen hello Windows Disk Manager. Om automount inte aktiveras som rekommenderas i kapitel [inställningen automount för anslutna diskar][planning-guide-5.5.3], hello nyligen kopplade volym > måste toobe tas online och initieras.
>
> ![Linux][Logo_Linux] Linux
>
> Du behöver tooinitialize en nyligen ansluten tom disk som beskrivs i [i den här artikeln][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Du måste också tooadd nya diskar toohello /etc/fstab.
>
>

- - -
### <a name="final-deployment"></a>Slutlig distribution
Hello slutlig distribution och specifika stegen, särskilt med angående toohello distribution av SAP utökad övervakning finns toohello [Deployment Guide][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Åtkomst till SAP-system som körs i virtuella Azure-datorer
Endast molnbaserad scenarier kanske du vill tooconnect toothose SAP-system över hello offentliga internet använda SAP-Gränssnittet. I dessa fall måste hello följande procedurer toobe tillämpas.

Senare i hello dokumentet diskuteras hello andra viktiga scenarier, anslutande tooSAP system i mellan lokala distributioner som har en plats-till-plats-anslutning (VPN-tunnel) eller Azure ExpressRoute-anslutning mellan Azure system och hello lokalt system.

### <a name="remote-access-toosap-systems"></a>Remote Access tooSAP system
Med Azure Resource Manager innehåller inga slutpunkter längre som hello tidigare klassiska modellen. Alla portar för en Azure ARM-VM är öppna så länge som:

1. Ingen Nätverkssäkerhetsgrupp har definierats för hello undernät eller hello nätverksgränssnitt. Nätverket trafik tooAzure virtuella datorer kan vara skyddad via så kallade ”Nätverkssäkerhetsgrupper”. Mer information finns i [vad är en Nätverkssäkerhetsgrupp (NSG)?][virtual-networks-nsg]
2. Någon Azure belastningsutjämnare har definierats för hello nätverksgränssnittet   

Se hello arkitektur skillnaden mellan klassiska modellen och ARM som beskrivs i [i den här artikeln][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-hello-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Konfigurationen av hello SAP-systemet och SAP GUI-anslutning för endast molnbaserad scenario
Finns den här artikeln som beskriver information toothis avsnittet: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Ändra brandväggsinställningar inom VM
Det kan vara nödvändigt tooconfigure hello brandväggen på din virtuella datorer tooallow för inkommande trafik tooyour SAP-system.

- - -
> ![Windows][Logo_Windows] Windows
>
> Hello Windows-brandväggen i en distribuerad virtuell dator i Azure är aktiverat som standard. Nu måste tooallow hello SAP Port toobe öppnas, annars hello SAP GUI kommer inte att kunna tooconnect.
> toodo detta:
>
> * Öppna kontrollpanelen\system och säkerhet\windows-brandväggen too'Advanced inställningar.
> * Nu ska du högerklicka på regler för inkommande trafik och välj ”ny regel'.
> * I hello valde följande guide toocreate en ny regel 'Port'.
> * I hello nästa steg i guiden hello, lämnar du hello inställning på TCP- och typ i hello portnummer du vill tooopen. Eftersom vår SAP instans-ID är 00, tog vi 3200. Om din instans har en annan instans., bör hello port du definierat tidigare baserat på hello instansnummer öppnas.
> * I hello nästa del av hello guiden måste tooleave hello objektet ”Tillåt anslutningen' som markerad.
> * I hello nästa steg i guiden hello måste toodefine om hello regeln gäller för domän, privata och offentliga nätverk. Justera det om nödvändigt tooyour måste. Ansluta till SAP GUI från hello utanför via hello offentligt nätverk, behöver du dock toohave hello regel som tillämpas toohello offentligt nätverk.
> * I hello sista steget hello guiden namn hello regeln och spara genom att trycka på ”Slutför”
>
> hello regeln träder i kraft omedelbart.
>
> ![Definitionen av port][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> hello Linux bilder i hello Azure Marketplace aktiverar inte hello iptables brandväggen som standard och hello anslutning tooyour SAP-systemet ska fungera. Om du har aktiverat iptables eller en annan brandvägg finns i iptables toohello dokumentationen eller hello används brandväggen tooallow inkommande tcp-trafik för port 32xx (där xx är hello system antalet SAP-system).
>
>

- - -
#### <a name="security-recommendations"></a>Säkerhetsrekommendationer
hello SAP GUI ansluter inte omedelbart tooany hello SAP instanser (port 32xx) som körs, men först ansluter via hello porten är öppen toohello SAP meddelandet serverprocessen (port 36xx). I hello tidigare hello samma port användes av servern för hello-meddelande för hello intern kommunikation toohello programinstanser. tooprevent lokalt programservrar oavsiktligt kommunicerar med en message-server i Azure hello interna kommunikationsportar kan ändras. Det rekommenderas starkt toochange hello intern kommunikation mellan hello SAP message-servern och dess program instanser tooa annat portnummer på datorer som har varit klonad från lokalt system, t.ex en klon av utveckling för projektet testning osv. Detta kan göras med hello standardparametern profil:

> rdisp/msserv_internal
>
>

enligt beskrivningen i [säkerhetsinställningar för hello SAP Message Server](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Begreppet endast molnbaserad distribution av SAP-instanser
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Enskild virtuell dator med SAP NetWeaver demo/utbildning scenario
![Kör enskild VM SAP demo-system med hello samma VM-namn, separat i Azure Cloud Services][planning-guide-figure-1700]

I det här scenariot (finns i kapitlet [endast molnbaserad] [ planning-guide-2.1] i det här dokumentet) vi implementerar ett scenario med vanliga utbildning-demon system där hello fullständig utbildning-demon scenario finns i en enda virtuell dator. Vi förutsätter att hello distributionen görs via avbildningen mallar för Virtuella datorer. Vi förutsätter också att flera av dessa demo/utbildningar virtuella datorer måste toobe som distribueras med hello virtuella datorer med hello samma namn.

hello antas att du har skapat en VM-avbildning som beskrivs i vissa avsnitt i kapitel [förbereda virtuella datorer med SAP för Azure] [ planning-guide-5.2] i det här dokumentet.

hello sekvens av händelser tooimplement hello scenariot ser ut så här:

##### <a name="powershell"></a>PowerShell
* Skapa en ny resursgrupp för varje utbildning/demo liggande

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```
* Skapa ett nytt lagringskonto om du inte vill toouse hanterade diskar

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Skapa ett nytt virtuellt nätverk för varje utbildning/demo liggande tooenable hello användning av hello samma värdnamn och IP-adresser. hello virtuellt nätverk är skyddat av en Nätverkssäkerhetsgrupp som bara tillåter trafik tooport 3389 tooenable åtkomst via fjärrskrivbord och port 22 för SSH.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Skapa en ny offentlig IP-adress som kan använda tooaccess hello virtuell dator från hello internet

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Skapa ett nytt nätverksgränssnitt för hello virtuell dator

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Skapa en virtuell dator. Hello endast molnbaserad scenariot för varje virtuell dator har hello samma namn. hello SAP-SID för hello SAP NetWeaver instanser i dessa virtuella datorer kommer att hello samma samt. Inom hello Azure-resursgrupp, hello namnet på hello VM behöver toobe unika, men olika Azure-resursgrupper kan du köra virtuella datorer med hello samma namn. Hej standardkonto för Windows-administratörer' eller 'rot' för Linux är inte giltiga. Därför måste ett nytt användarnamn för administratören toobe definierats tillsammans med ett lösenord. hello storleken på hello VM måste också toobe som definierats.

```powershell
#####
# Create a new virtual machine with an official image from hello Azure Marketplace
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains hello private image that you want toouse
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path tooVHD that contains hello OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path tooVHD that contains hello OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Du kan också lägga till ytterligare diskar och återställa nödvändigt innehåll. Tänk på att alla blob-namn (URL: er toohello blobbar) måste vara unikt i Azure.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM

# Optional: Attach additional Managed Disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzureRmVM
```

##### <a name="cli"></a>CLI
Följande exempelkod hello kan användas på Linux. För Windows, antingen Använd PowerShell enligt beskrivningen ovan eller anpassa hello exempel toouse % rgName % i stället för $rgName och ange hello miljövariabeln hello Windows kommandot *ange*.

* Skapa en ny resursgrupp för varje utbildning/demo liggande

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Skapa ett nytt lagringskonto

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Skapa ett nytt virtuellt nätverk för varje utbildning/demo liggande tooenable hello användning av hello samma värdnamn och IP-adresser. hello virtuellt nätverk är skyddat av en Nätverkssäkerhetsgrupp som bara tillåter trafik tooport 3389 tooenable åtkomst via fjärrskrivbord och port 22 för SSH.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Skapa en ny offentlig IP-adress som kan använda tooaccess hello virtuell dator från hello internet

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Skapa ett nytt nätverksgränssnitt för hello virtuell dator

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Skapa en virtuell dator. Hello endast molnbaserad scenariot för varje virtuell dator har hello samma namn. hello SAP-SID för hello SAP NetWeaver instanser i dessa virtuella datorer kommer att hello samma samt. Inom hello Azure-resursgrupp, hello namnet på hello VM behöver toobe unika, men olika Azure-resursgrupper kan du köra virtuella datorer med hello samma namn. Hej standardkonto för Windows-administratörer' eller 'rot' för Linux är inte giltiga. Därför måste ett nytt användarnamn för administratören toobe definierats tillsammans med ett lösenord. hello storleken på hello VM måste också toobe som definierats.

```
#####
# Create virtual machines using storage accounts
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password

#####
# Create virtual machines using Managed Disks
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
```

```
#####
# Create a new virtual machine with a VHD that contains hello private image that you want toouse
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path tooimage vhd>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path tooimage vhd> --authentication-type password

#####
# Create a new virtual machine with a Managed Disk Image
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id> --authentication-type password
```

* Du kan också lägga till ytterligare diskar och återställa nödvändigt innehåll. Tänk på att alla blob-namn (URL: er toohello blobbar) måste vara unikt i Azure.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Mall
Du kan använda hello exempelmallarna i hello azure snabbstartsmallar databasen på github.

* [Enkel virtuell Linux-dator](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Enkla Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Virtuell dator från avbildningen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-toocommunicate-within-azure"></a>Implementera en uppsättning virtuella datorer som behöver toocommunicate i Azure
Det här scenariot endast molnbaserad är ett typiskt scenario för träning och demo där hello programvara som representerar hello demo/utbildning scenariot sträcker sig över flera virtuella datorer. hello olika komponenter hello olika virtuella datorer måste toocommunicate med varandra. Igen, i det här scenariot någon lokal nätverksanslutning eller scenarion med flera lokaler krävs.

Det här scenariot är en utökning av hello-installation som beskrivs i kapitlet [enskild virtuell dator med SAP NetWeaver demo/utbildning scenariot] [ planning-guide-7.1] i det här dokumentet. I det här fallet läggs flera virtuella datorer tooan befintlig resursgrupp. I hello består följande exempel hello utbildning liggande av en SAP ASCS/SCS virtuell dator, en virtuell dator som kör en DBMS och en SAP Application Server-instans VM.

Innan du skapar det här scenariot måste toothink om grundläggande inställningar som redan utförs i hello scenariot innan.

#### <a name="resource-group-and-virtual-machine-naming"></a>Resursgrupp och namngivning av virtuell dator
Alla resursgruppnamn måste vara unika. Utveckla egna namngivningsschemat för dina resurser, exempelvis `<rg-name`>-suffix.

hello namn på virtuell dator har toobe som är unika inom hello resursgrupp.

#### <a name="set-up-network-for-communication-between-hello-different-vms"></a>Konfigurera nätverk för kommunikation mellan hello olika virtuella datorer
![Uppsättning virtuella datorer i Azure-nätverk][planning-guide-figure-1900]

tooprevent namnkonflikter med kloner av hello samma utbildning/demo landskap toocreate ett Azure Virtual Network måste för varje liggande. DNS-namnmatchningen kommer att tillhandahållas av Azure eller konfigurera egna DNS-server utanför Azure (inte toobe ytterligare beskrivs här). I det här scenariot konfigurerar vi inte våra egna DNS. Kommunikation via värdnamn vara aktiverat för alla virtuella datorer i ett Azure Virtual Network.

hello varför tooseparate utbildning eller demo landskap av virtuella nätverk och inte bara resursen grupper kan vara:

* hello SAP liggande som ställs in måste egna AD/OpenLDAP och en domänserver behov toobe del av varje hello landskap.  
* Hej SAP liggande som konfigurera har komponenter som måste toowork med fasta IP-adresser.

Mer information om virtuella Azure-nätverk och hur toodefine dem finns i [i den här artikeln][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Distribuera SAP virtuella datorer med företagets nätverksanslutning (mellan platser)
Du kör en SAP-liggande och vill att toodivide hello distributionen mellan minimalt för avancerade DBMS-servrar, lokala virtualiserade miljöer för programmet lager och mindre 2-skikts konfigurerat SAP-system och Azure IaaS. hello antas grundläggande att SAP system inom en SAP liggande måste toocommunicate med varandra och många andra programkomponenter som distribuerats i hello företag, oberoende av deras distribution form. Det bör också finnas några skillnader som introducerades av hello distribution formulär för hello användare ansluta till SAP-Gränssnittet eller andra gränssnitt. Dessa villkor kan endast vara uppfyllda när vi har hello lokala Active Directory/OpenLDAP och DNS-tjänster utökat toohello Azure system via plats-till-plats/flera plats-anslutning eller privata anslutningar som Azure ExpressRoute.

I ordning tooget mer background på hello implementeringsinformation om SAP på Azure, vi rekommenderar att du tooread kapitel [begrepp Cloud-Only distribution av SAP instanser] [ planning-guide-7] i det här dokumentet som beskriver Vissa av hello grunderna skapar i Azure och hur de ska användas med SAP-program i Azure.

### <a name="scenario-of-an-sap-landscape"></a>Scenario för en SAP-liggande
hello kan scenarion med flera lokaler ungefär beskrivas som i hello graphics nedan:

![Plats-till-plats-anslutning mellan lokala och Azure tillgångar][planning-guide-figure-2100]

hello scenariot ovan beskriver ett scenario där hello lokala AD/OpenLDAP och DNS är utökat tooAzure. Ett visst IP-adressintervall är reserverat per Azure-prenumeration på hello lokalt sida. hello IP-adressintervall tilldelas tooan Azure Virtual Network på hello Azure sida.

#### <a name="security-considerations"></a>Säkerhetsöverväganden
hello Minimikravet är hello användning av säker kommunikationsprotokoll som SSL/TLS för webbläsaråtkomst eller VPN-baserade anslutningar för att komma åt toohello Azure services. hello antas företag att hantera hello VPN-anslutning mellan sina företagets nätverk och Azure väldigt annorlunda. Vissa företag kanske blankly öppna alla hello-portar. Vissa andra företag kanske vill toobe mycket exakt i vilka portar som de behöver tooopen osv.

I hello tabellen nedan vanliga SAP visas kommunikationsportar. Det är i princip tillräcklig tooopen hello SAP-gateway-port.

| Tjänst | Portnamn | Exempel `<nn`> = 01 | Standardintervallet (min max) | Kommentar |
| --- | --- | --- | --- | --- |
| Dispatcher |sapdp`<nn>` finns * |3201 |3200 – 3299 |SAP Dispatcher, används av Java och SAP GUI för Windows |
| Meddelande-server |sapms`<sid`> finns ** |3600 |ledigt sapms`<anySID`> |SID = SAP-System-ID |
| Gateway |sapgw`<nn`> finns * |3301 |Ledigt |SAP-gateway som används för CPIC och RFC kommunikation |
| SAP-router |sapdp99 |3299 |Ledigt |Endast CI (central instans) Tjänstnamn kan omtilldelas i /etc/services tooan godtyckligt värde efter installationen. |

*) nn = SAP-instansnummer

*) sid = SAP-System-ID

Mer detaljerad information om portar som behövs för olika SAP produkter eller tjänster som SAP produkter hittar du här <http://scn.sap.com/docs/DOC-17124>.
Med det här dokumentet ska kunna tooopen dedikerad portar i hello VPN-enheten som krävs för specifika SAP-produkter och scenarier.

Andra säkerhetsmetoder när du distribuerar virtuella datorer i ett sådant scenario kan vara toocreate en [Nätverkssäkerhetsgruppen] [ virtual-networks-nsg] toodefine åtkomstregler.

### <a name="dealing-with-different-virtual-machine-series"></a>Med en annan virtuell dator-serie
Under hello senaste 12 månaderna Microsoft lagt till många fler VM-typer som skiljer sig något i antal vCPUs, minne eller viktigare på maskinvara som den körs på. Inte alla dessa virtuella datorer stöds med SAP (se stöds VM typer i SAP-kommentar [1928533]). Vissa av dessa virtuella datorer körs på olika värden maskinvara generationer. Dessa värden maskinvara generationer distribueras om i hello granularitet för en Azure-Skalningsenhet. Sätt fall kan uppstå om hello hello olika VM-storlek du valt inte kan köras på samma Skalningsenhet. En Tillgänglighetsuppsättning är begränsad i hello möjlighet toospan skalenheter baserat på olika typer av maskinvara.  För exempel om du vill toorun hello DBMS på A5 A11 virtuella datorer och hello SAP programnivå på virtuella datorer G-serien, du kan tvingas toodeploy ett enda SAP-system eller annan SAP-system inom olika Tillgänglighetsuppsättningar.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Skriva ut på en lokal nätverksskrivare från SAP-instans i Azure
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Utskrift över TCP/IP i scenarion med flera lokaler
Ställa in din lokala TCP/IP baserat nätverksskrivare i en Azure VM är övergripande hello desamma som företagets nätverk, förutsatt att du har en VPN plats-till-plats-tunneln eller ExpressRoute-anslutning upprättas.

- - -
> ![Windows][Logo_Windows] Windows
>
> toodo detta:
>
> * Vissa nätverksskrivare har en konfigurationsguide som gör det enkelt tooset skrivare i en Azure VM. Om inga guiden program har distribuerats med skrivare hello ”manuell” hur tooset hello skrivare är toocreate en ny TCP/IP-skrivarport.
> * Öppna Kontrollpanelen -> enheter och skrivare > Lägg till en skrivare
> * Välj Lägg till en skrivare med hjälp av en TCP/IP-adress eller värdnamn
> * Skriv i hello IP-adressen för hello skrivare
> * Skrivarport standard 9100
> * Om det behövs installerar du hello lämplig skrivardrivrutin manuellt.
>
> ![Linux][Logo_Linux] Linux
>
> * som för Windows just Följ hello standardproceduren tooinstall nätverksskrivare
> * Följ bara hello offentliga Linux guider för [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) eller [Red Hat och Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) om hur tooadd en skrivare.
>
>

- - -
![Nätverket utskrift][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Värdbaserad skrivare över SMB (delad skrivare) i scenarion med flera lokaler
Värdbaserad skrivare är inte nätverks-kompatibla avsiktligt. Men en värdbaserad skrivare kan delas mellan datorer i ett nätverk som hello skrivaren är anslutna tooa slås på datorn. Ansluta ditt företags nätverk plats-till-plats eller ExpressRoute och delar en lokal skrivare. hello SMB-protokollet använder NetBIOS i stället för DNS som namntjänst. hello NetBIOS-värdnamn kan skilja sig från hello DNS-värdnamn. hello standard fall har hello NetBIOS-värdnamn och hello DNS-värdnamn är identiska. hello DNS-domänen gör inte mening i utrymmet för hello NetBIOS-namn. Därför hello fullständigt kvalificerade DNS-värdnamn som består av hello DNS-värdnamn och DNS-domänen får inte användas i utrymmet för hello NetBIOS-namn.

hello skrivarresursen identifieras med ett unikt namn i hello nätverk:

* Värdnamnet för hello SMB-Server (alltid behövs).
* Namn på hello resurs (alltid behövs).
* Namnet på hello domän om skrivarresursen inte hello samma domän som SAP-system.
* Dessutom kan det vara nödvändiga tooaccess hello skrivarresursen mellan ett användarnamn och ett lösenord.

Anvisningar:

- - -
> ![Windows][Logo_Windows] Windows
>
> Dela en lokal skrivare.
> Öppna hello Utforskaren och anger hello resursnamn för hello skrivare i hello Azure VM.
> Installationsguiden för skrivare hjälper dig hello installationsprocessen.
>
> ![Linux][Logo_Linux] Linux
>
> Här följer några exempel på dokumentation om hur du konfigurerar nätverksskrivare i Linux eller inklusive kapitel rörande utskrift i Linux. Den fungerar hello samma sätt i en Azure Linux-dator så länge hello VM är en del av en VPN-anslutning:
>
> * SLES <_Share_or_Windows_Share https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba)>
> * RHEL eller Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB-skrivare (skrivare vidarebefordran)
Tootheir lokal skrivare enheter i en fjärrsession är inte tillgängligt i Azure hello möjligheten för hello Fjärrskrivbordstjänster tooprovide användare hello åtkomst.

- - -
> ![Windows][Logo_Windows] Windows
>
> Mer information om utskrift i Windows finns här: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integrering av SAP Azure system till korrigering och transportsystem (Proportionerlig) mellan platser
Hej SAP ändrings- och Transport System (Proportionerlig) måste toobe konfigurerats tooexport och importera begäran över system i hello liggande. Vi förutsätter att hello development instanser av ett SAP-system (utveckling) finns i Azure hello quality assurance (QA) och produktiva system (PRD) är lokalt. Vi förutsätter dessutom att det finns en central transport-katalog.

##### <a name="configuring-hello-transport-domain"></a>Konfigurera hello Transport domän
Konfigurera din domän Transport på hello system som du angav som hello Transport domänkontrollant enligt beskrivningen i [konfigurera hello Transport domänkontrollant](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). En systemanvändare TMSADM kommer att skapas och hello krävs för RFC målet kommer att skapas. Du kan kontrollera anslutningarna RFC med hello transaktion SM59. Värdnamnsmatchning måste aktiveras i transport-domän.

Anvisningar:

* I vårt exempel har vi beslutat hello lokalt QAS system blir hello CTS domänkontrollanten. Anropa transaktion STM. hello dialogruta Proportionerlig. En dialogruta för Konfigurera Transport domän visas. (Den här dialogrutan visas bara om du ännu inte har konfigurerat en transport-domän.)
* Kontrollera att användaren hello skapas automatiskt TMSADM auktoriseras (SM59 -> ABAP anslutning -> TMSADM@E61.DOMAIN_E61 -> information -> Utilities(M) auktorisering Test ->). hello första skärmen transaktion STM ska visa att SAP systemet nu fungerar som domänkontrollant hello i hello transport domän som visas här:

![Första skärmen transaktion STM på hello-domänkontrollant][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-hello-transport-domain"></a>Inklusive SAP-system i hello Transport domän
hello-sekvens, inklusive ett SAP-system i en domän för transport ser ut som följer:

* På hello DEV system i Azure, gå toohello transportsystem (klient 000) och anropa transaktion STM. Välj andra konfiguration från hello dialogrutan och fortsätt med inkluderar System i domänen. Ange hello-domänkontrollant som målvärden ([inklusive SAP-system i hello Transport domän](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). hello systemet är nu väntar toobe ingår i hello transport domän.
* Av säkerhetsskäl måste har du sedan toogo tillbaka toohello domain controller tooconfirm din begäran. Välj Systemöversikt och Godkänn hello väntar system. Bekräfta hello fråga- och hello konfiguration kommer att distribueras.

SAP systemet innehåller nu hello nödvändig information om alla hello andra SAP-system i hello transport domän. Vid hello samma tid, hello adressen data hello nya SAP systemets skickas tooall hello andra SAP-system och hello SAP-systemet har angetts i profilen för hello transport av kontrollprogrammet för hello transport. Kontrollera om RFC: er och åtkomst toohello transport hello domänens directory fungerar.

Fortsätt med hello konfiguration transport som vanligt beskrivs i dokumentationen för hello [ändrings- och Transport System](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Anvisningar:

* Kontrollera att din STM lokalt är korrekt konfigurerad.
* Kontrollera att hello värdnamnet för hello Transport domänkontrollant kan lösas genom den virtuella datorn på Azure och vice visa.
* Anropet transaktion STM -> andra Configuration -> inkluderar System i domänen.
* Bekräfta hello anslutning i hello på lokala Proportionerlig system.
* Konfigurera transport vägar, grupper och lager som vanligt.

I plats-till-plats anslutna mellan lokala vara scenarier, hello fördröjning mellan lokala och Azure fortfarande betydande. Om vi följer hello aktivitetssekvensen för att transportera objekt genom tooproduction för utveckling och testning system eller tänka på att tillämpa transporter eller support paket toohello olika system, inser du, beroende på hello plats hello centrala transport katalog, vissa hello system uppstår fördröjningar läsning eller skrivning av data i hello centrala transport directory. hello situationen är liknande tooSAP liggande konfigurationer där hello olika system sprids via olika datacenter med stor avstånd mellan hello datacenter.

I ordning toowork runt sådana svarstid och ha hello system fungerar Snabb läsning eller skrivning tooor från hello transport directory du kan ställa in två STM transport domäner (en för lokala. och en med hello system i Azure och länka hello transport domäner Kontrollera den här dokumentationen som beskriver hello principerna bakom detta begrepp i hello SAP Proportionerlig: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/ 38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Anvisningar:

* Konfigurera en transport domän på varje plats (lokalt och Azure) med transaktion STM <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Länka hello domäner med en länk i domänen och bekräfta hello länken mellan hello två domäner.
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Distribuera hello konfigurationssystemet toohello länkad.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC trafik mellan SAP-instanser som finns i Azure och lokala (mellan platser)
RFC trafik mellan datorer som finns lokalt och i Azure måste toowork. tooset en anslutning anropar transaktion SM59 i ett källsystem där du behöver toodefine en RFC anslutning mot hello målsystemet. hello-konfigurationen är liknande toohello standardinställningen av en RFC-anslutning.

Vi förutsätter att hello virtuella datorer som kör SAP-system som behöver toocommunicate med varandra som finns i i hello scenarion med flera lokaler, hello samma domän. Därför hello installationen av en RFC-anslutning mellan SAP-system skiljer sig inte från hello konfigurationsstegen och indata i lokala scenarier.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Åtkomst till ”lokal' fileshares från SAP-instanser som finns i Azure eller tvärtom
SAP-instanser som finns i Azure måste tooaccess filresurser som är inom hello företagets lokaler. Lokal SAP-instanser måste dessutom tooaccess filresurser som finns i Azure. tooenable hello filresurser måste du konfigurera hello behörigheter och delningsalternativ på hello lokalt system. Se till att tooopen hello portar på hello VPN eller ExpressRoute-anslutning mellan Azure och ditt datacenter.

## <a name="supportability"></a>Support
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure övervakningslösning för SAP
I ordning tooenable hello övervakning av uppdrag kritiska SAP-system på Azure hello SAP hämta övervakningsverktyg SAPOSCOL eller SAP Värdagenten data från hello Azure virtuella tjänstvärden via ett Azure-övervakning tillägg för SAP. Eftersom hello krav av SAP var mycket specifik tooSAP program, Microsoft valt inte toogenerically implementera hello som krävs för funktioner i Azure, men lämna för kunder toodeploy hello nödvändiga övervakning komponenter och konfigurationer tootheir Virtuella datorer som körs i Azure. Dock automatiskt distribution och livscykeln för hantering av hello övervaka komponenter främst av Azure.

#### <a name="solution-design"></a>Lösningsdesign
hello lösning utvecklat tooenable SAP är baserat på hello arkitekturen i Azure VM-agenten och ramverket. hello uppfattning om hello Azure VM-agenten och tillägget framework är tooallow installation av programvara program tillgängliga i galleriet för hello Azure VM-tillägget på en virtuell dator. Hej princip idé bakom det här konceptet är tooallow (i sådana fall hello Azure övervakning tillägget för SAP), hello distribution av särskilda funktioner i den virtuella dator och hello-konfigurationen av sådan programvara vid tidpunkten för distribution.

hello 'Azure VM Agent' som möjliggör hantering av specifika Azure VM-tillägg i hello VM är injekteras i virtuella Windows-datorer som standard på Skapa en virtuell dator i hello Azure-portalen. Hello VM-agenten är redan en del av Azure Marketplace-avbildning vid SUSE, Red Hat eller Oracle Linux. Om någon vill överföra en Linux VM från har lokala tooAzure hello VM-agenten toobe installerats manuellt.

Hej grundläggande byggstenarna för hello övervakning lösning i Azure för SAP ser ut så här:

![Tillägg för Microsoft Azure-komponenter][planning-guide-figure-2400]

I hello blockdiagram ovan visas en del av hello övervakningslösning SAP finns i hello Azure VM-avbildning och Azure-tillägg-galleriet som är en globalt replikerad databas som hanteras av Azure-åtgärder. Det är hello ansvar hello gemensamma SAP/MS-teamet som arbetar på hello Azure implementering av SAP toowork med Azure Operations toopublish nya versioner av hello Azure övervakning tillägget för SAP.

När du distribuerar en ny Windows VM läggs automatiskt hello Azure VM-agenten i hello VM. hello-funktionen för den här agenten är toocoordinate hello inläsning och konfiguration av hello Azure-tillägg för övervakning av SAP NetWeaver-system. För Linux virtuella datorer är redan hello Azure VM-agenten som en del av hello Azure Marketplace OS-avbildningen.

Det finns ett steg som återstår toobe som körs av hello kunden. Detta är hello aktivering och konfiguration av hello insamling av prestanda. hello processen relaterade toohello konfiguration sker automatiskt efter en PowerShell-skript eller CLI-kommando. hello PowerShell-skript kan hämtas i hello Microsoft Azure Script Center enligt beskrivningen i hello [Deployment Guide][deployment-guide].

hello övergripande arkitektur i hello Azure övervakningslösning SAP ser ut:

![Azure övervakningslösning för SAP NetWeaver][planning-guide-figure-2500]

**För hello exakt hur-tooand detaljerade anvisningar för att använda dessa PowerShell-cmdlets eller CLI kommandot under distributioner, följer du instruktionerna i hello hello [Deployment Guide][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integrering av Azure finns SAP-instans i SAProuter
SAP-instanser som körs i Azure måste toobe tillgänglig från SAProuter samt.

![SAP-Router-nätverksanslutning][planning-guide-figure-2600]

En SAProuter aktiverar hello TCP/IP-kommunikation mellan deltagande system om det finns ingen direkt IP-anslutning. Detta ger hello fördelen att någon slutpunkt till slutpunkt-anslutning mellan hello kommunikation partner krävs på nätverksnivån. Hej SAProuter lyssnar på port 3299 som standard.
tooconnect SAP instanser via en SAProuter behöver du toogive hello SAProuter sträng och värdnamn med eventuella försök tooconnect.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Hittills hello fokus för hello dokumentet har SAP NetWeaver i allmänhet eller hello SAP NetWeaver ABAP stacken. I det här små avsnittet listas särskilda överväganden för hello SAP Java-stacken. En av hello viktigaste SAP NetWeaver Java uteslutande baserad program är hello SAP Enterprise Portal. Andra SAP NetWeaver baserat program som SAP PI och SAP lösning Manager använder både hello SAP NetWeaver ABAP och Java-stackar. Det verkligen är därför en behovet tooconsider särskilda aspekter relaterade toohello SAP NetWeaver Java-stack samt.

### <a name="sap-enterprise-portal"></a>SAP Enterprise Portal
hello installationen av en SAP-Portal i en virtuell Azure-dator skiljer sig inte från en på lokal installation om du distribuerar i mellan lokala scenarier. Eftersom hello DNS gör du genom att lokalt, kan du göra hello portinställningar hello enskilda instanser som konfigurerade lokalt. hello rekommendationer och begränsningar som beskrivs i det här dokumentet hittills gäller i allmänhet för ett program som SAP Enterprise Portal eller hello SAP NetWeaver Java-stacken.

![Exponerade SAP-portalen][planning-guide-figure-2700]

En särskild distributionsscenariot som vissa kunder är hello direkt exponering av hello SAP Enterprise Portal toohello Internet men hello virtuell värd är ansluten toohello företagets nätverk via plats-till-plats VPN-tunnel eller ExpressRoute. Scenario har du toomake till att vissa portar är öppna och inte blockeras av en brandvägg eller säkerhetsgrupp. hello måste samma säkerhetsnivån toobe tillämpas när du vill tooconnect tooan SAP Java-instans från lokalt i en situation med endast molnet.

hello inledande portal URI är http (s):`<Portalserver`>: 5XX00/irj där hello port utgörs av 50000 plus (Systemnumber × 100). hello standard portal URI SAP systemet 00 är `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. Mer information, ta en titt på <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Slutpunktskonfiguration][planning-guide-figure-2800]

Kontrollera den här dokumentationen om du vill toocustomize hello URL och/eller portar för SAP Enterprise Portal:

* [Ändra Portal-URL](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Ändra standardportnumren Portal-portnummer](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Hög tillgänglighet och Disaster Recovery (DR) för SAP NetWeaver körs på Azure Virtual Machines
### <a name="definition-of-terminologies"></a>Definition av termer som gäller
hello termen **hög tillgänglighet (HA)** är vanligtvis relaterade tooa en uppsättning tekniker som minimerar IT avbrott genom att tillhandahålla affärskontinuitet av IT-tjänster via redundanta, feltolerant eller failover skyddade komponenter inuti hello **samma** datacenter. I vårt fall, inom en Azure-Region.

**Katastrofåterställning (DR)** är också på minimera avbrott i IT-tjänster och deras återställning men tvärs över **olika** datacenter som vanligtvis finns hundratals kilometer direkt. I vårt fall vanligtvis mellan olika Azure-regioner inom hello samma geopolitiska region eller som upprättas av du som kund.

### <a name="overview-of-high-availability"></a>Översikt över hög tillgänglighet
Vi kan avgränsa hello diskussion om SAP hög tillgänglighet i Azure i två delar:

* **Hög tillgänglighet för Azure-infrastrukturen**, till exempel hög tillgänglighet för beräkning (VM), nätverk, lagring etc. och dess fördelar för att öka tillgängligheten för SAP-program.
* **Hög tillgänglighet för SAP-program**, till exempel hög tillgänglighet för SAP programvarukomponenter:
  * SAP-programservrar
  * SAP ASCS/SCS-instans
  * DB-server

och hur den kan kombineras med Azure-infrastrukturen hög tillgänglighet.

SAP hög tillgänglighet i Azure har vissa skillnader i jämförelse tooSAP hög tillgänglighet i en lokal fysisk eller virtuell miljö. hello följande dokumentet från SAP beskriver standardkonfigurationer SAP hög tillgänglighet i virtualiserade miljöer i Windows: <http://scn.sap.com/docs/DOC-44415>. Det finns ingen sapinst-integrerade SAP-HA konfiguration för Linux som om det finns för Windows. Om SAP HA lokalt för Linux hitta mer information här: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure-infrastrukturen med hög tillgänglighet
Det finns en enda VM SLA för 99,9%. tooget en idé hur hello tillgängligheten för en enda virtuell dator kan se ut som om du bara kan skapa hello produkten av hello olika tillgängliga Azure SLA: <https://azure.microsoft.com/support/legal/sla/>.

hello grunden för beräkning av hello är 30 dagar i månaden, eller 43200 minuter. Därför 0,05% avbrottstid motsvarar too21.6 minuter. Hello tillgängligheten för hello olika tjänster kommer som vanligt, multiplicera i hello följande sätt:

(Tillgänglighet Service #1/100) * (tillgänglighet Service #2/100) * (tillgänglighet Service #3/100) *...

Exempel:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 eller 99.75% övergripande tillgänglighet.

#### <a name="virtual-machine-vm-high-availability"></a>Virtuell dator (VM) med hög tillgänglighet
Det finns två typer av händelser i Azure-plattformen som kan påverka hello tillgänglighet för de virtuella datorerna: planerat underhåll och oplanerat underhåll.

* Planerat underhållshändelser är periodiska uppdateringar som görs av Microsoft toohello underliggande Azure-plattformen tooimprove övergripande tillförlitlighet, prestanda och säkerhet för hello plattformsinfrastruktur som de virtuella datorerna körs på.
* Oplanerat underhållshändelser inträffar när hello maskinvara eller fysisk infrastruktur underliggande din virtuella dator fel har uppstått på något sätt. Det kan vara lokala nätverksfel, lokala diskfel eller andra fel på racknivå. När sådant fel identifieras hello Azure-plattformen automatiskt att migrera den virtuella datorn från hello ohälsosamt fysisk server värd för din virtuella tooa felfri fysisk server. Sådana händelser är sällsynt, men kan även orsakas av din virtuella tooreboot.

Mer information finns i den här dokumentationen: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure Storage-redundans
hello data i Microsoft Azure Storage-konto är alltid replikerade tooensure hållbarhet och hög tillgänglighet, uppfyller hello Azure Storage SLA även i hello sida av tillfälliga maskinvarufel.

Eftersom Azure Storage är att ha tre bilder hello data som standard, RAID5 eller RAID1 över flera Azure-diskar är inte nödvändigt.

Mer information finns i den här artikeln: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-tooachieve-higher-availability-of-sap-applications"></a>Använda Azure-infrastrukturen VM starta om tooAchieve ”högre tillgänglighet” för SAP-program
Om du inte toouse funktioner som Windows Server Failover Clustering WSFC- eller Pacemaker på Linux (för närvarande stöds endast för SLES 12 och högre), starta om Azure VM är utnyttjade tooprotect en SAP-systemet mot planerade och oplanerade driftstopp för hello Azure fysisk server-infrastrukturen och övergripande underliggande Azure-plattformen.

> [!NOTE]
> Det är viktigt toomention att starta om Azure VM främst skyddar virtuella datorer och inte program. Starta om VM inte ger hög tillgänglighet för SAP-program, men den erbjuder en viss nivå av infrastrukturens tillgänglighet och därför indirekt ”högre tillgänglighet” för SAP-system. Det finns också några SLA för hello tid det tar toorestart en virtuell dator efter en planerad eller oplanerad värden avbrott. Den här metoden för 'med hög tillgänglighet är därför inte lämpar sig för viktiga komponenter av en SAP-system som (A) SCS eller DBMS.
>
>

Ett annat viktigt infrastruktur-element för hög tillgänglighet är lagring. Exempelvis är Azure Storage-SLA 99,9% tillgänglighet. Om en distribuerar du alla virtuella datorer med dess diskar till en enda Azure Storage-konto, potentiella Azure Storage otillgänglighet kommer att placeras i den Azure Storage-konto på alla virtuella datorer och även alla SAP komponenter som körs i dessa virtuella datorer.  

I stället för att alla virtuella datorer till en enda Azure Storage-konto, du kan också använda dedikerade konton för varje virtuell dator och på så sätt öka övergripande VM och SAP tillgänglighet genom att använda flera oberoende Azure Storage-konton.

Azure-hanterade diskar placeras i hello Feldomän för hello virtuell dator som de är kopplade till. Om du placerar två virtuella datorer i en tillgänglighetsgrupp och använder hanterade diskar, hello plattform tar hand om distribuerar hello hanterade diskar till olika Feldomäner samt. Om du planerar toouse Premium-lagring, rekommenderar vi använder också hantera diskar.

En exempel-arkitekturen för en SAP NetWeaver system som använder Azure-infrastrukturen hög tillgänglighet och storage-konton kan se ut så här:

![Använda Azure-infrastrukturen ”högre” tillgänglighet för hög tillgänglighet tooachieve SAP-program][planning-guide-figure-2900]

En exempel-arkitekturen för en SAP NetWeaver system som använder Azure-infrastrukturen hög tillgänglighet och hanterade diskar kan se ut så här:

![Använda Azure-infrastrukturen ”högre” tillgänglighet för hög tillgänglighet tooachieve SAP-program][planning-guide-figure-2901]

För kritiska SAP-komponenter uppnås hello hittills följande:

* Hög tillgänglighet för SAP-programservrar (AS)

  SAP-programserverinstanser är redundanta komponenter. Varje SAP som instansen har distribuerats på egen virtuell dator som körs i en annan Azure-fel och uppgradera domän (se kapitlen [Feldomäner] [ planning-guide-3.2.1] och [uppgradera domäner][planning-guide-3.2.2]). Detta säkerställs med hjälp av Azure-Tillgänglighetsuppsättningar (finns i kapitlet [Azure-Tillgänglighetsuppsättningar][planning-guide-3.2.3]). Potentiella planerad eller oplanerad otillgänglighet en Azure-fel eller uppgradera domän kommer inte finns ett begränsat antal virtuella datorer med deras SAP instanser.

  Varje SAP eftersom instansen är placerad i sin egen Azure Storage-konto – potentiella otillgänglighet ett Azure Storage-konto kommer att endast en virtuell dator med dess SAP instans. Men tänk på att det finns en gräns på Azure Storage-konton inom en Azure-prenumeration. tooensure automatisk start av (A) SCS-instans efter hello VM omstart kontrollerar du tooset hello Autostart parameter i instansen (A) SCS profil beskrivs i kapitlet [med hjälp av Autostart för SAP instanser] [ planning-guide-11.5].
  Läs även kapitel [hög tillgänglighet för SAP-programservrar] [ planning-guide-11.4.1] för mer information.

  Även om du använder hanterade diskar dessa diskar lagras också i ett Azure Storage-konto och kan inte tillgängliga i en händelse av ett avbrott för lagring.

* *Högre* tillgänglighet för SAP (A) SCS-instans

  Vi använder här Azure VM starta om tooprotect hello VM med installerade SAP (A) SCS-instans. I hello fall av planerad eller oplanerad driftstopp på Azure-servrarna, virtuella datorer kommer att startas om på en annan server. Som tidigare nämnts kan starta om Azure VM främst skyddar virtuella datorer och inte program, i det här fallet hello (A) SCS-instans. Via hello VM starta om ska vi nå indirekt ”högre tillgänglighet” för SAP (A) SCS-instansen. tooinsure automatisk start av (A) SCS-instans efter hello VM omstart kontrollerar du tooset Autostart parameter i instansen (A) SCS profil beskrivs i kapitlet [med hjälp av Autostart för SAP instanser][planning-guide-11.5]. Det innebär att hello (A) SCS-instans som en enskilda felpunkter (SPOF) körs i en enda virtuell dator är hello bestämmande faktor hello tillgänglighet för hello hela SAP liggande.

* *Högre* tillgängligheten för DBMS-Server

  Här, liknande toohello SAP (A) SCS instans användningsfall, vi använda Azure VM starta om tooprotect hello VM med installerad programvara för DBMS och vi uppnår ”högre tillgänglighet” av DBMS programvara via virtuella datorn startar om.
  DBMS som körs i en enda virtuell dator är också en SPOF och det är hello bestämmande faktor hello tillgänglighet för hello hela SAP liggande.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP hög tillgänglighet på Azure IaaS
tooachieve fullständig SAP system med hög tillgänglighet måste tooprotect alla viktiga SAP-systemkomponenter, för exempel redundant SAP programservrar och unika komponenter (till exempel enskilda felpunkter) som SAP (A) SCS-instans och DBMS.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Hög tillgänglighet för SAP-programservrar
Hello SAP programinstanser dialogrutan och servrar är det inte nödvändigt toothink om en lösning för specifika hög tillgänglighet. Hög tillgänglighet bara uppnås genom redundans och därmed ha tillräckligt många dem i olika virtuella datorer. De ska alla placeras i samma Tillgänglighetsuppsättning för Azure tooavoid som hello virtuella datorer kan uppdateras på hello hello samtidigt under planerat underhåll driftstopp. hello grundläggande funktioner som bygger på olika uppgradering och Feldomäner i en Azure-Skalningsenhet introducerades redan i kapitel [uppgradera domäner][planning-guide-3.2.2]. Azure Tillgänglighetsuppsättningar presenteras i kapitlet [Azure-Tillgänglighetsuppsättningar] [ planning-guide-3.2.3] i det här dokumentet.

Det finns inget obegränsat antal fel och uppgradera domäner som kan användas av en Azure Tillgänglighetsuppsättning inom en Skalningsenhet för Azure. Det innebär att försätta ett antal virtuella datorer i en Tillgänglighetsuppsättning, förr eller senare mer än en virtuell dator avslutas med hello samma fel eller uppgradera domän.

Distribuera några SAP-programserver instanser i deras dedikerade virtuella datorer och förutsatt att vi fem uppgradera domäner hello följande bild visar sig hello slutet. Hej max antal fel och uppdatera domäner inom en tillgänglighetsuppsättning kan ändras i framtida hello:

![Hög tillgänglighet för SAP programservrar i Azure][planning-guide-figure-3000]

Mer information finns i den här dokumentationen: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-hello-sap-ascs-instance-on-windows"></a>Hög tillgänglighet för hello SAP (A) SCS instans i Windows
Failover Cluster WSFC (Windows Server) är en lösning för vanliga tooprotect hello SAP (A) SCS-instans. Det är även integrerad i sapinst i form av ett ”hög tillgänglighet-installation”. Vid denna tidpunkt är hello Azure-infrastrukturen inte kan tooprovide hello funktioner tooset in hello som krävs för Windows Server Failover Cluster hello samma sätt som det är klart lokalt.

Januari 2016 ger inte hello Azure molnplattform kör hello Windows hello möjligheten att använda en delad klustervolym på en disk som delas mellan två virtuella Azure-datorer.

En giltig lösning är dock hello användning av 3 tillverkare som ger en delad volym av synkrona och transparent diskreplikering som kan integreras i WSFC. Den här metoden innebär att endast aktiva hello klusternoden är kan tooaccess en hello disk kopierar vid en viss tidpunkt. Konfigurationen är stöds tooprotect hello SAP (A) SCS instans på Windows gästoperativsystemet på Azure Virtual Machines i kombination med 3 tillverkare SIOS DataKeeper januari 2016 denna hög tillgänglighet.

Hej SIOS DataKeeper lösningen innehåller en delad disk klustret resurs tooWindows Failover-kluster genom att:

* En ytterligare Azure virtuell Hårddisk ansluten tooeach hello virtuella datorer (VM) som finns i en konfiguration för Windows-kluster
* SIOS DataKeeper Cluster Edition körs på båda noderna för VM
* Har konfigurerats på ett sätt som synkront speglar hello innehållet i hello ytterligare SIOS DataKeeper Cluster Edition VHD som är kopplad volym från virtuella datorer tooadditional virtuell Hårddisk ansluten källvolymen av virtuell dator.
* SIOS DataKeeper abstrahera hello käll- och lokala volymer och presentera dem tooWindows redundanskluster som en enda delad disk.

Du kan hitta all information om hur tooinstall ett Windows-redundanskluster med SIOS DataKeeper och SAP i hello [Clustering SAP ASCS-instans med redundanskluster för Windows Server på Azure med SIOS DataKeeper] [ ha-guide-classic]vitboken.

#### <a name="high-availability-for-hello-sap-ascs-instance-on-linux"></a>Hög tillgänglighet för hello SAP (A) SCS-instansen på Linux
Från och med Dec 2015 finns även ingen motsvarande tooshared disk WSFC för Linux virtuella datorer i Azure. Alternativa lösningar med hjälp av 3 tillverkare som SIOS för Windows verifieras inte ännu för att köra SAP på Linux på Azure.

#### <a name="high-availability-for-hello-sap-database-instance"></a>Hög tillgänglighet för hello SAP-databasinstans
hello SAP DBMS HA standardinstallation baseras på två DBMS virtuella datorer där DBMS hög tillgänglighet funktionen används tooreplicate data från hello active DBMS instans toohello andra VM till en passiv DBMS-instans.

Hög tillgänglighet och katastrofåterställning recovery funktioner för DBMS i allmänna samt specifika DBMS beskrivs i hello [DBMS Deployment Guide][dbms-guide].

#### <a name="end-to-end-high-availability-for-hello-complete-sap-system"></a>Hög tillgänglighet från slutpunkt till slutpunkt för hello hela SAP-systemet
Här är två exempel på en fullständig SAP NetWeaver HA arkitektur i Azure - en för Windows och en för Linux.

Ohanterad endast: hello begreppen som förklaras nedan behöva toobe komprometteras lite när du distribuerar många SAP-system och hello antal distribuerade virtuella datorer överskrider hello maxgränsen Lagringskonton per prenumeration. I sådana fall måste virtuella hårddiskar för virtuella datorer toobe kombineras i ett Lagringskonto. Vanligtvis vill du göra det genom att kombinera virtuella hårddiskar för SAP programnivå virtuella datorer i olika SAP-system.  Vi också kombinera olika virtuella hårddiskar på en annan DBMS virtuella datorer i olika SAP-system i en Azure Storage-konto. Därmed Tänk på hur hello IOPS-gränser för Azure Storage-konton (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] Hög tillgänglighet i Windows
![SAP NetWeaver programarkitektur hög tillgänglighet med SQLServer i Azure IaaS][planning-guide-figure-3200]

följande Azure konstruktioner hello används för hello SAP NetWeaver system, toominimize effekten av infrastrukturen problem och värden korrigering:

* hello hela systemet distribueras på Azure (obligatoriskt – DBMS layer, (A) SCS-instans och hela appen layer måste toorun i hello samma plats).
* hello hela systemet körs inom en Azure-prenumeration (krävs).
* hello hela systemet körs inom ett virtuellt Azure-nätverk (krävs).
* hello avgränsning av hello virtuella datorer i en SAP-systemet till tre Tillgänglighetsuppsättningar är möjligt med alla hello virtuella datorer som tillhör toohello samma virtuella nätverk.
* Alla virtuella datorer som kör DBMS instanser av en SAP-systemet är i en Tillgänglighetsuppsättning. Vi förutsätter att det finns fler än en virtuell dator som kör DBMS instanser per system eftersom interna DBMS-hög tillgänglighet som används för funktioner, t.ex. SQL Server AlwaysOn eller Oracle Data Guard.
* Alla virtuella datorer som kör DBMS instanser använda sina egna storage-konto. DBMS data och loggfilen filer replikeras från ett konto tooanother lagring lagringskonto med hjälp av DBMS hög tillgänglighet funktioner som synkroniserar hello-data. Inte finns en storage-konto kommer att en SQL-Windows-klusternod, men inte hello hela SQL Server-tjänsten.
* Alla virtuella datorer som kör (A) SCS-instans av en SAP-systemet är i en Tillgänglighetsuppsättning. En Failover Cluster WSFC (Windows Server) har konfigurerats i de virtuella datorerna tooprotect hello (A) SCS-instansen.
* Alla virtuella datorer (A) SCS-instanser körs använder sina egna lagringskonto. (A) SCS instansfiler och SAP globala mappen replikeras från ett konto tooanother lagring lagringskonto med SIOS DataKeeper replikering. Otillgänglighet ett lagringskonto kommer inte finns något (A) SCS klusternoden, men inte hello hela (A) SCS-tjänsten.
* ALLA hello virtuella datorer som representerar hello SAP programnivå server finns i en tredje Tillgänglighetsuppsättning.
* ALLA hello virtuella datorer som kör SAP-programservrar använder sina egna lagringskonto. Inte finns en storage-konto kommer att en server för SAP-program, där andra SAP AS fortsätta toorun.

hello följande figur illustrerade hello samma liggande med hjälp av hanterade diskar.

![SAP NetWeaver programarkitektur hög tillgänglighet med SQLServer i Azure IaaS][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] Hög tillgänglighet på Linux
hello-arkitekturen för SAP hög tillgänglighet på Linux i Azure är i praktiken hello desamma som för Windows som beskrivs ovan. Jan 2016 det finns ingen SAP (A) SCS HA lösning stöds ännu på Linux på Azure

Följaktligen som januari 2016 ett Azure-SAP-Linux-system kan uppnå hello samma tillgänglighet som en SAP Windows Azure-system på grund av saknade hög tillgänglighet för hello (A) SCS-instans och hello single instance SAP ASE databas.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Med hjälp av Autostart för SAP-instanser
SAP erbjuds hello funktioner toostart SAP instanser omedelbart efter hello start hello OS inom hello VM. Hej hur finns dokumenterade i SAP Knowledge Base-artikel [1909114]. Dock SAP inte rekommenderar toouse hello längre inställningen eftersom det finns ingen kontroll hello efter instansen startas om, under förutsättning att mer än en virtuell dator har fått påverkas eller flera instanser körde per virtuell dator. Under förutsättning att ett typiskt Azure scenario för en SAP application server-instansen i ett VM- och hello en enda virtuell dator så småningom få startas, hello Autostart är inte mycket viktig och kan aktiveras genom att lägga till den här parametern:

    Autostart = 1

Starta profil för hello SAP ABAP och/eller Java-instansen till hello.

> [!NOTE]
> hello Autostart parameter kan ha vissa downfalls samt. I större detalj hello hello parametern utlösare start av en SAP ABAP eller Java-instans när hello relaterade Windows-/ Linux-tjänsten för hello-instansen har startats. Som är visserligen hello fallet när hello operativsystemet startas. Dock omstarter av SAP-tjänster finns också en gemensam sak för SAP programvara livscykelhantering funktioner som SUMMAN eller andra uppdateringar eller uppgraderingar. En instans toobe startas om automatiskt i alla förväntas dessa funktioner inte. Hello Autostart parameter ska vara inaktiverat innan du kör sådana uppgifter. hello Autostart parametern bör också inte användas för SAP-instanserna är klustrade som CI-ASCS/SCS.
>
>

Se ytterligare information om autostart för SAP instanser här:

* [Starta/Stoppa SAP tillsammans med din Unix Server Starta/Stoppa](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Starta och stoppa SAP NetWeaver Hanteringsagenter](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Hur tooenable automatisk Start av HANA-databas](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Större 3-skikts SAP-system
Hög tillgänglighet aspekter av 3-skikts SAP konfigurationer fick beskrivs tidigare redan. Men hur system där hello DBMS-serverkrav är för stor toohave finns den i Azure, men hello SAP programnivå kan distribueras till Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>Platsen för 3-skikts SAP-konfigurationer
Det är inte stöds toosplit hello programmet nivå sig själv eller hello program och DBMS nivå mellan lokala och Azure. En SAP-systemet är antingen helt distribuerade lokalt eller i Azure. Det är inte heller stöds toohave vissa hello programservrar köras lokalt och vissa andra i Azure. Det är hello börja med hello diskussion. Vi stöder inte toohave hello DBMS komponenterna i en SAP-system och hello SAP server programnivå distribueras i två olika Azure-regioner. Till exempel DBMS i USA, västra och SAP programnivå i centrala USA. Orsaken till inte har stöd för sådana konfigurationer är hello latens känslighet hello SAP NetWeaver arkitektur.

Dock över hello kursens förra året data center partner som har utvecklats samtidigt platser tooAzure regioner. Dessa samtidigt platser är ofta i Azure data för mycket nära toohello fysiska resurser i en Azure-Region. hello korta avstånd och anslutning av tillgångar i hello samplacering via ExpressRoute till Azure kan resultera i en latens som är mindre än 2 MS. I sådana fall går toolocate hello DBMS layer (inklusive lagring SAN/NAS) i sådan samplacering och hello SAP programnivå i Azure. Från och med Dec 2015 har vi inte alla distributioner som de som. Men olika kunder med icke-SAP application-distributioner använder redan dessa metoder.

### <a name="offline-backup-of-sap-systems"></a>Offline säkerhetskopiering av SAP-system
Beroende på hello SAP-konfigurationen valt (2 eller 3-nivåer) det kan vara en behöver tooback upp. hello innehåll hello VM själva plus toohave en säkerhetskopia av hello-databasen. hello DBMS-relaterade säkerhetskopior är förväntade toobe göras med metoderna i databasen. En detaljerad beskrivning av hello olika databaser, finns i [DBMS guiden][dbms-guide]. Hej på andra sidan, hello SAP data kan säkerhetskopieras på ett sätt som offline (inklusive hello databasen samt innehåll) enligt beskrivningen i det här avsnittet online eller som beskrivs i nästa avsnitt om hello.

Hej offlinesäkerhetskopiering kräver i praktiken en avstängning av hello VM via hello Azure-portalen och en kopia av hello VM basdisk samt alla anslutna diskar toohello VM. Detta skulle bevara en punkt i tiden bild av hello VM och dess associerade disk. Det rekommenderas toocopy hello ”säkerhetskopiering” i en annan Azure Storage-konto. Därför hello proceduren som beskrivs i kapitlet [kopierar diskar mellan Azure Storage-konton] [ planning-guide-5.4.2] i det här dokumentet skulle tillämpas.
Förutom hello avstängning med hjälp av hello Azure portal en kan också göra det via Powershell eller CLI som beskrivs här: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

En återställning av det aktuella tillståndet skulle består av att ta bort hello grundläggande VM samt som hello ursprungliga diskar hello basera VM och monterade diskar, kopiera tillbaka hello sparade diskar toohello ursprungliga Storage-konto eller resurs gruppen för hanterade diskar och omdistribuera hello system.
Den här artikeln visar ett exempel hur tooscript detta bearbetning i Powershell: <http://www.westerndevs.com/azure-snapshots/>

Kontrollera att tooinstall en ny SAP-licens eftersom återställer en säkerhetskopiering av VM som beskrivs ovan skapar en ny maskinvarunyckel.

### <a name="online-backup-of-an-sap-system"></a>Onlinesäkerhetskopiering för en SAP-system
Säkerhetskopiering av hello DBMS utförs med DBMS-specifika metoder som beskrivs i hello [DBMS guiden][dbms-guide].

Andra virtuella datorer i hello SAP-system kan säkerhetskopieras med funktioner för säkerhetskopiering av Azure virtuella datorer. Säkerhetskopiering av Azure virtuella datorer har fått introduceras tidigt i 2015 och under tiden är en standardmetod tooback av en fullständig VM i Azure. Azure-säkerhetskopiering lagrar hello säkerhetskopieringar i Azure och gör en återställning av en virtuell dator igen.

> [!NOTE]
> Från och med Dec 2015 med säkerhetskopiering behålls inte hello unika VM-ID som används för SAP-licensiering. Detta innebär att en återställning från en säkerhetskopiering kräver installation av en ny nyckel för SAP-licens som hello återställts VM anses toobe en ny virtuell dator och inte en ersättning för det tidigare som har sparats.
>
> ![Windows][Logo_Windows] Windows
>
> Teoretiskt sett kan virtuella datorer som kör databaser kan säkerhetskopieras på ett konsekvent sätt om hello DBMS system stöder hello Windows VSS (Volume Shadow Copy Service <https://msdn.microsoft.com/library/windows/desktop/bb968832 (v=vs.85).aspx >) som, till exempel SQL Server har.
> Tänk dock som baseras på Virtuella Azure-säkerhetskopieringar i tidpunkt återställer databaser inte är möjligt. Därför vi rekommenderar tooperform säkerhetskopior av databaser med DBMS-funktioner i stället för att förlita dig på Azure VM Backup.
>
> tooget bekant med säkerhetskopiering av Azure virtuella datorer starta här: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Andra kan toouse en kombination av Microsoft Data Protection Manager har installerats i en virtuell dator i Azure och Azure Backup för databaser för säkerhetskopiering/återställning. Mer information hittar du här: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Det finns inga motsvarande tooWindows VSS i Linux. Endast filkonsekventa säkerhetskopior är därför möjligt men inte programkonsekvent säkerhetskopiering. hello SAP DBMS säkerhetskopieringen ska göras med hjälp av DBMS-funktioner. hello filsystem som innehåller hello SAP-relaterad data kan sparas, till exempel använder tar som beskrivs här: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure som DR-plats för produktion SAP landskap
Eftersom Mid 2014 aktivera tillägg toovarious komponenterna för Hyper-V, System Center och Azure hello användning av Azure som DR-plats för virtuella datorer som körs lokalt baserat på Hyper-V.

En blogg med information om hur toodeploy den här lösningen är dokumenteras här: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Sammanfattning
hello huvudpunkter med hög tillgänglighet för SAP-system i Azure är:

* Vid denna tidpunkt, hello SAP felpunkt kan inte skyddas exakt hello samma sätt som kan utföras i lokala distributioner. hello beror på att delade diskkluster ännu inte kunde genereras i Azure utan hello 3 tillverkare.
* Du måste toouse DBMS-funktioner som inte är beroende av delad disk klustret teknik för hello DBMS lager. Information som finns dokumenterade i hello [DBMS guiden][dbms-guide].
* toominimize hello effekten av problem inom Feldomäner i hello Azure-infrastruktur eller värden underhåll, bör du använda Azure-Tillgänglighetsuppsättningar:
  * Det rekommenderas toohave en Tillgänglighetsuppsättning för hello SAP programnivå.
  * Det rekommenderas toohave som en separat Tillgänglighetsuppsättning för hello SAP DBMS lager.
  * Det rekommenderas inte tooapply hello samma tillgänglighetsuppsättning för virtuella datorer i olika SAP-system.
  * Det rekommenderas toouse hanteras Premiumdiskar.
* För säkerhetskopiering hello SAP DBMS layer, kontrollera hello [DBMS guiden][dbms-guide].
* Säkerhetskopiera SAP dialogrutan instanser är lite meningsfullt eftersom det är normalt snabbare tooredeploy enkel dialogruta instanser.
* Säkerhetskopierar hello VM som innehåller hello global katalog hello SAP-systemet och med den alla hello profiler för olika instanser hello särskilt meningsfullt och bör utföras med Windows Säkerhetskopiering eller, till exempel tar på Linux. Eftersom det är skillnad mellan Windows Server 2008 (R2) och Windows Server 2012 (R2) som gör det enklare tooback med hello senare Windows Server släpper rekommenderar vi toorun Windows Server 2012 (R2) som gästoperativsystem för Windows.
