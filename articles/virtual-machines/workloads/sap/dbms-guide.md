---
title: "aaaAzure distribution av virtuella datorer DBMS för SAP NetWeaver | Microsoft Docs"
description: "Azure virtuella datorer DBMS-distribution för SAP NetWeaver"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5654dac7-4204-4387-b312-3d8b2898eb3a
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 501f6fbc2baa379b706e95d2bfba377ac129b382
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>Azure virtuella datorer DBMS-distribution för SAP NetWeaver
[767598 ]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
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
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
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
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

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
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
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

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den här handboken är en del av hello dokumentation om implementering och distribuera hello SAP-programvara i Microsoft Azure. Innan du läser handboken läsa hello [Implementeringsguide för planering och][planning-guide]. Det här dokumentet beskriver hello distribution av olika relationell databas Management Systems (RDBMS) och relaterade produkter i kombination med SAP på Microsoft Azure virtuella datorer (VM) med hello Azure-infrastrukturen som en tjänst (IaaS)-funktioner.

hello papper kompletterar hello SAP dokumentationen och SAP anteckningar, som representerar hello primära resurser för installation och distribution av program på anges plattformar.

## <a name="general-considerations"></a>Allmänna överväganden
I det här kapitlet introduceras överväganden SAP-relaterade DBMS system som körs i virtuella Azure-datorer. Det finns några referenser toospecific DBMS system i det här kapitlet. I stället hanteras hello specifika DBMS system i det här dokumentet, efter det här kapitlet.

### <a name="definitions-upfront"></a>Definitioner förskott
I hello dokumentet använder vi hello följande villkor:

* IaaS: Infrastruktur som en tjänst.
* PaaS: Plattform som en tjänst.
* SaaS: Programvara som en tjänst.
* SAP komponent: ett enskilt SAP program, till exempel ECC BW, lösning Manager eller EP.  SAP-komponenter kan baseras på traditionell ABAP eller Java-teknik eller icke-NetWeaver baserat program, till exempel Business-objekt.
* SAP-miljö: en eller flera komponenter för SAP logiskt grupperade tooperform funktioner, till exempel utveckling, QAS, träning, DR eller produktion.
* SAP liggande: Refererar detta toohello hela SAP tillgångar i kundens IT-miljön. hello SAP liggande innehåller alla produktions- och icke-produktionsmiljöer.
* SAP System: hello kombination av DBMS lager- och programnivå av, till exempel en SAP ERP-utvecklingssystemet, SAP BW testsystemet, SAP CRM produktionssystem osv. I Azure-distributioner är det inte stöds toodivide dessa två lager mellan lokala och Azure. Detta innebär en SAP-systemet är antingen distribueras lokalt eller den har distribuerats i Azure. Du kan dock distribuera hello olika system för en SAP liggande i Azure eller lokalt. Du kan till exempel distribuera hello SAP CRM-utveckling och testning system i Azure som hello SAP CRM produktion system lokalt.
* Endast molnbaserad distribution: en distribution där hello Azure-prenumeration inte är ansluten via ett plats-till-plats eller ExpressRoute-anslutning toohello lokala nätverksinfrastruktur. Gemensam dokumentation för Azure dessa typer av distributioner beskrivs också som 'endast molnbaserad-distributioner. Virtuella datorer distribueras med den här metoden kan nås via hello Internet och den offentliga Internet slutpunkter tilldelade toohello virtuella datorer i Azure. hello lokala Active Directory (AD) och DNS är inte utökat tooAzure i dessa typer av distributioner. Därför är hello virtuella datorer inte en del av hello lokala Active Directory. Obs: Endast molnbaserad distributioner i det här dokumentet har definierats som slutförts SAP natur, som körs i Azure utan utökning av Active Directory eller namnmatchning från lokala i offentliga moln. Endast molnbaserad konfigurationer stöds inte för SAP produktionssystem eller konfigurationer där SAP STM eller andra lokala resurser måste toobe används mellan SAP-system som finns på Azure och resurser som finns lokalt.
* Anslutningar mellan lokala: Beskriver ett scenario där virtuella datorer som är distribuerade tooan Azure-prenumeration som har plats-till-plats, flera platser eller ExpressRoute-anslutning mellan hello lokalt datacenter(s) och Azure. Gemensamma Azure-dokumentationen dessa typer av distributioner beskrivs också som mellan lokala scenarier. hello orsaken till hello anslutning är tooextend lokala domäner lokala Active Directory och DNS-lokalt till Azure. hello lokalt liggande är utökade toohello Azure tillgångar hello prenumeration. Med det här tillägget, kan hello virtuella datorer vara en del av hello lokala domän. Domänanvändare av hello lokal domän kan komma åt hello servrar och kan köra services på de virtuella datorer (till exempel DBMS services). Kommunikation och namnmatchning mellan virtuella datorer distribueras lokala och virtuella datorer som distribueras i Azure är möjligt. Vi räknar toobe hello vanligaste scenariot för att distribuera SAP tillgångar på Azure. Mer information finns i [i den här artikeln] [ vpn-gateway-cross-premises-options] och [i den här artikeln][vpn-gateway-site-to-site-create].

> [!NOTE]
> Anslutningar mellan lokala distributioner av SAP-system där Azure virtuella datorer som kör SAP-system är medlemmar i en lokal domän stöds för produktion SAP-system. Anslutningar mellan lokala konfigurationer stöds för att distribuera delar eller slutföra SAP landskap till Azure. Kör även hello fullständig SAP liggande i Azure kräver med dessa virtuella datorer som en del av lokal domän och ANNONSER. I tidigare versioner av hello dokumentationen talat om Hybrid-IT-scenarier där hello termen ”Hybrid' är rotad i hello faktum att det finns en anslutning mellan Azure och lokalt. I det här fallet ”Hybrid' också innebär att hello virtuella datorer i Azure är en del av hello lokala Active Directory.
> 
> 

Vissa Microsoft-dokumentationen beskriver anslutningar mellan lokala scenarier lite annorlunda särskilt för DBMS HA konfigurationer. Hello gäller hello SAP-relaterade dokument handlar hello mellan lokala scenario bara toohaving ett plats-till-plats eller privat (ExpressRoute) anslutning och toohello faktum att hello SAP liggande fördelas mellan lokala och Azure.

### <a name="resources"></a>Resurser
hello är följande guider tillgängliga för hello avsnittet SAP distributioner i Azure:

* [Azure virtuella datorer planering och implementering för SAP NetWeaver][planning-guide]
* [Distribution av Azure virtuella datorer för SAP NetWeaver][deployment-guide]
* [Azure virtuella datorer DBMS-distribution för SAP NetWeaver (det här dokumentet)][dbms-guide]

hello följande SAP anteckningar är relaterade toohello avsnittet SAP i Azure:

| Nummer | Rubrik |
| --- | --- |
| [1928533] |SAP-program i Azure: produkter och Virtuella Azure-typer |
| [2015553] |SAP på Microsoft Azure: stöd för krav |
| [1999351] |Felsökning av utökad Azure övervakning för SAP |
| [2178632] |Key som övervakning mått för SAP på Microsoft Azure |
| [1409604] |Virtualisering på Windows: förbättrad övervakning |
| [2191498] |SAP på Linux med Azure: förbättrad övervakning |
| [2039619] |SAP-program på Microsoft Azure med hello Oracle-databas: produkter och versioner |
| [2233094] |DB6: SAP-program på Azure med hjälp av IBM DB2 för Linux, UNIX- och Windows - ytterligare Information |
| [2243692] |Linux på Microsoft Azure (IaaS) VM: SAP problem |
| [1984787] |SUSE LINUX Enterprise Server 12: Installationsinformation |
| [2002167] |Red Hat Enterprise Linux 7.x: Installation och uppgradering |
| [2069760] |Oracle Linux 7.x SAP Installation och uppgradering |
| [1597355] |Växlingsutrymme rekommendation för Linux |
| [2171857] |Oracle Database 12c - filen stöd på Linux |
| [1114181] |Oracle Database 11g - filen stöd på Linux |


Också läsa hello [Tillståndsändringsavisering Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) som innehåller alla SAP anteckningar för Linux.

Du bör ha kunskaper om hello Microsoft Azure-arkitektur och hur Microsoft Azure virtuella datorer distribueras och drivas. Du hittar mer information på <https://azure.microsoft.com/documentation/>

> [!NOTE]
> Vi kan **inte** diskutera Microsoft Azure-plattformen som en tjänst (PaaS)-erbjudanden för hello Microsoft Azure-plattformen. Det här dokumentet handlar om hur du kör ett databashanteringssystem (DBMS) i Microsoft Azure Virtual Machines (IaaS) på samma sätt som du skulle köra hello DBMS i din lokala miljö. Databasen funktioner och funktionalitet mellan dessa två erbjudanden skiljer sig väldigt mycket och bör inte blandas med varandra. Se även: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Eftersom vi diskuterar IaaS är i allmänhet hello Windows, Linux och DBMS installation och konfiguration i stort sett hello samma som en virtuell dator eller bare metal datorn installerar du lokalt. Det finns dock vissa arkitektur och systemhantering implementering beslut, som skiljer sig när du använder IaaS. hello syftet med det här dokumentet är tooexplain hello specifika arkitektur och system management skillnader som du måste förberedas för när du använder IaaS.

I allmänhet hello övergripande områden av skillnaden som beskrivs i det här dokumentet är:

* Planera hello rätt VM/disklayouten för SAP system tooensure har hello rätt data filens layout och kan få tillräckligt med IOPS för din arbetsbelastning.
* Nätverk överväganden när du använder IaaS.
* Viss databas funktioner toouse i toooptimize hello layout för databasen.
* Överväganden för säkerhetskopiering och återställning i IaaS.
* Använda olika typer av avbildningar för distribution.
* Hög tillgänglighet i Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktur för en RDBMS-distribution
Ordna toofollow detta kapitel, är det nödvändigt toounderstand vad angavs i [detta] [ deployment-guide-3] kapitel i hello [Deployment Guide] [ deployment-guide]. Kunskap om hello annan VM-serien och deras skillnader och skillnader i Azure Standard och Premium-lagring ska förstå och kända innan du läser det här kapitlet.

Diskar som innehåller ett operativsystem har begränsad too127 GB i storlek tills mars 2015. Den här begränsningen har lyfts i mars 2015 (för mer information kontrollen <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). Därifrån på diskar som innehåller operativsystemet för hello kan ha hello storlek samma som de andra disk. Vi föredrar dock fortfarande en struktur för distribution där hello operativsystemet, DBMS och eventuell SAP-binärfiler skiljer sig från hello-databasfiler. Vi förvänta dig därför SAP-system som körs i Azure Virtual Machines har hello grundläggande VM (eller disk) installeras med hello operativsystemet, databasen management system körbara filer och SAP körbara filer. hello DBMS data och loggfiler lagras i Azure-lagring (Standard eller Premium-lagring) i separata diskar och anslutna som logiska diskar toohello ursprungliga Azure operativsystemavbildningen VM. 

Beroende på utnyttja Azure Standard eller Premium-lagring (till exempel med hjälp av hello DS-serien eller GS-serien VMs) det finns andra kvoter i Azure, som finns dokumenterade [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows]. När du planerar din disklayouten, behöver du toofind hello bästa balansen mellan hello kvoter för hello följande objekt:

* hello antalet datafiler.
* hello antal diskar som innehåller hello-filer.
* hello IOPS kvoter för en enskild disk.
* hello datagenomströmning per disk.
* hello antal ytterligare hårddiskar som är möjligt per VM-storlek.
* hello totala genomflödet för en virtuell dator kan ge.

Azure tillämpar en kvot för IOPS per datadisk. Dessa kvoter är olika för diskar som finns i Azure standardlagring och Premium-lagring. I/o-svarstiderna är också mycket olika mellan hello två typer av lagring med Premium-lagring och leverera faktorer bättre svarstider för i/o. Varje hello VM av olika typer har ett begränsat antal datadiskar som du är kan tooattach. En annan begränsning är att bara vissa VM-typer kan utnyttja Azure Premium-lagring. Det innebär att hello beslut för en viss typ av virtuell dator inte kan endast styrs av hello CPU och minne, utan även av hello IOPS, svarstid och disk genomströmning krav som vanligtvis skalas med hello antal diskar eller hello typ av Premium-lagring diskar. Särskilt med Premium-lagring kan hello storleken på en disk också avgöras genom hello antal IOPS och genomflöde som behöver toobe uppnås genom att varje disk.

hello faktum att hello övergripande IOPS hastighet, hello antalet diskar som monterats, och hello storleken på hello VM alla kopplas samman, kan leda till en Azure-konfiguration för en SAP system toobe annat än dess lokala distributionen. hello IOPS-gränser per LUN kan vanligtvis konfigureras i lokala distributioner. Med Azure Storage är fast eller som ett Premium-lagring beroende på typ av disk hello dessa begränsningar. Med lokala distributioner visas så kundkonfigurationer för databasservrar som använder många olika volymer för särskilda körbara filer som SAP och hello DBMS eller särskilda volymer för tillfälliga databaser eller tabell blanksteg. När sådana ett lokalt system har flyttats tooAzure leda tooa skräp potentiella IOPS bandbredd genom att spara en disk för körbara filer eller databaser som inte utför någon eller inte mycket IOPS. I Azure Virtual Machines rekommenderar vi därför att hello DBMS och SAP körbara filer installeras på hello OS-disk om möjligt.

hello ska placering hello-databasfilerna och loggfilerna och hello typ av Azure Storage används, definieras av IOPS, svarstid och genomströmning krav. I ordning toohave tillräckligt med IOPS för hello transaktionsloggen kanske du framtvingad tooleverage flera diskar för hello transaktionsloggen filen eller Använd en större disk i Premium-lagring. I sådana fall en skulle skapa en programvara RAID (till exempel Windows Storage Pool för Windows eller MDADM och LVM (logisk volym Manager) för Linux) med hello-diskar som innehåller hello transaktionsloggen.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Enhet D:\ i en Azure VM är en icke-beständig enhet som backas upp av vissa lokala diskar på hello Azure compute-nod. Det innebär att alla ändringar som gjorts toohello innehåll på hello D:\ enheten går förlorad när hello VM startas eftersom den är icke-beständig. Med ”ändringar” menar vi sparade filer, kataloger som skapas, program och så vidare.
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux virtuella Azure-datorer ansluta automatiskt en enhet i /mnt/resource som är en icke-beständig enhet som backas upp av lokala diskar på hello Azure compute-nod. Det innebär att alla ändringar som gjorts toocontent i /mnt/resource försvinner när hello VM startas eftersom den är icke-beständig. Vi menar av ändringar, filer som sparats, kataloger som skapas, program och så vidare.
> 
> 

- - -
Beroende på hello Azure VM-serien, hello lokala diskar på hello compute-nod visas olika prestanda, vilket kan kategoriseras som:

* A0 A7: Mycket begränsad prestanda. Inte användas för något utöver växlingsfil för windows
* A8-A11: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning
* D-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning
* DS-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning
* G-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning
* GS-serien: Goda prestandaegenskaper med vissa tiotusen IOPS och > 1 GBIT/s genomströmning

Uttryck ovanför kopplar toohello VM typer som är certifierade med SAP. hello VM-serien med utmärkt IOPS och genomströmning uppfyller kraven för att dra nytta av vissa DBMS-funktioner, t.ex. tempdb eller temporärt tabellutrymme.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Cachelagring för virtuella datorer och diskar
När vi skapar datadiskar via hello-portalen eller när vi montera överförda diskar tooVMs väljer vi om hello i/o-trafik mellan hello VM och de diskar som finns i Azure storage cachelagras. Azure Standard och Premium-lagring kan du använda två olika tekniker för den här typen av cachen. I båda fallen är hello cache själva disken säkerhetskopieras på hello samma enheter används av hello tillfälliga disk (D:\ i Windows) eller /mnt/resource på Linux hello VM.

För Azure standardlagring är hello möjliga cache typer:

* Ingen cachelagring
* Läsa cachelagring
* Läsa och skriva cachelagring

I ordning tooget konsekvent och deterministisk prestanda bör du ange hello cachelagring på Azure standardlagring för alla diskar som innehåller **DBMS-relaterade datafiler, loggfiler och tabellen utrymme too'NONE'**. hello cachelagring av hello VM kan förbli med hello standard.

För Azure Premium Storage finns följande alternativ för cachelagring hello:

* Ingen cachelagring
* Läsa cachelagring

Rekommendation för Azure Premium-lagring är tooleverage **läsa cachelagring för filer** av hello SAP-databasen och välj **ingen cachelagring för hello diskar loggfilerna**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Programvarubaserad RAID
Som anges ovan måste toobalance hello antalet IOPS behövs för hello databasfilerna över hello antalet diskar kan du konfigurera och hello högsta IOPS som en virtuell dator i Azure ger per disk- eller Premium-lagring disktyp. Den enklaste sättet toodeal med hello IOPS ladda över diskar är toobuild en programvarubaserad RAID över hello olika diskar. Placera sedan ett antal datafiler i hello SAP DBMS på hello LUN högg utanför hello programvarubaserad RAID. Beroende på hello krav som du kanske vill tooconsider hello användning av Premium-lagring samt sedan två hello tre olika Premium-lagring diskar har högre IOPS kvot än diskar baserat på standardlagring. Förutom hello betydande bättre i/o-svarstid som tillhandahålls av Azure Premium-lagring. 

Samma gäller toohello transaktionsloggen hello olika DBMS-system. I många av dem bara att lägga till fler Tlog filer hjälper inte eftersom hello DBMS system skriva till en av hello filer på bara en gång. Om du behöver högre IOPS takt än en enda Standard lagring baserat disk kan leverera, du kan stripe över flera standardlagring diskar eller du kan använda en större Premium-lagring disktyp som utöver högre IOPS priser levererar även faktorer kortare svarstid för hello skrivning I/o i hello transaktionsloggen.

Situationer som uppstått i Azure-distributioner som skulle ge företräde åt med en programvarubaserad RAID är:

* Loggen/gör om transaktionsloggen kräver fler IOPS än Azure tillhandahåller för en enskild disk. Som nämnts ovan detta kan lösas genom att skapa ett LUN över flera diskar med hjälp av en programvarubaserad RAID.
* En ojämn i/o arbetsbelastning distribution över hello olika datafiler av hello SAP-databasen. I sådana fall kan en uppleva en datafil träffa hello kvot i stället ofta. Medan andra filer inte även får Stäng toohello IOPS kvoten för en enskild disk. I sådana fall hello enklaste lösningen är toobuild en LUN över flera diskar med hjälp av en programvarubaserad RAID. 
* Du inte vet vilka hello exakt i/o arbetsbelastning per datafilen är och bara ungefär vet vad hello övergripande IOPS arbetsbelastningen mot hello DBMS är. Enklaste toodo är toobuild en LUN med hello hjälp av en programvarubaserad RAID. hello summan av kvoter för flera diskar bakom denna LUN sedan ska uppfylla hello kända IOPS hastighet.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Vi rekommenderar att du använder Windows lagringsutrymmen om du kör på Windows Server 2012 eller senare. Det är effektivare än Windows Striping av tidigare versioner av Windows. Du kanske måste toocreate hello Windows lagringspooler och lagringsutrymmen med PowerShell-kommandon när du använder Windows Server 2012 som operativsystem. hello PowerShell-kommandon finns här <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Endast MDADM och LVM (hanteraren för logiska volym) är stöds toobuild en programvarubaserad RAID på Linux. Mer information finns i följande artiklar hello:
> 
> * [Konfigurera programvarubaserad RAID på Linux] [ virtual-machines-linux-configure-raid] (för MDADM)
> * [Konfigurera LVM på en virtuell Linux-dator i Azure][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Överväganden för att kunna utnyttja VM-serien, som kan toowork med Azure Premium-lagring vanligtvis är:

* Krav för i/o-latens som är Stäng toowhat SAN/NAS-enheter leverera.
* Begäran för faktorer bättre i/o-svarstid än Azure standardlagring kan ge.
* Högre IOPS per virtuell dator än vad som kan uppnås med flera standardlagring diskar mot en viss typ av virtuell dator.

Eftersom hello underliggande Azure Storage att varje disk tooat minst tre lagringsnoder, replikerar enkel RAID 0 striping kan användas. Det finns inget behov av tooimplement RAID5 eller RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage
Microsoft Azure Storage lagrar hello grundläggande VM (med OS) och diskar eller BLOB tooat minst tre separata lagringsnoder. När du skapar ett lagringskonto eller hanterade diskar, är det ett val av skydd som visas här:

![GEO-replikering är aktiverat för Azure Storage-konto][dbms-guide-figure-100]

Azure Storage lokal replikering (lokalt Redundant) ger skydd mot dataförlust på grund av fel tooinfrastructure att fåtal kunder kan ger toodeploy. Som visas ovanför det finns fyra olika alternativ med en femte som en variant av en av hello först tre. Titta närmare på dem kan vi skilja:

* **Premium lokalt Redundant lagring (LRS)**: Azure Premium Storage ger stöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens. Det finns tre replikeringar av hello data i hello samma Azure-datacenter för en Azure-region. hello kopiorna är i olika fel och uppgradera domäner (begrepp finns [detta] [ planning-guide-3.2] kapitlet i hello [Planeringshandboken][planning-guide]). En ny replik genereras automatiskt vid en replik av hello data gå ur funktion på grund av tooa lagring nodfel eller diskfel.
* **Lokalt Redundant lagring (LRS)**: I det här fallet finns tre replikeringar av hello data i hello samma Azure-datacenter för en Azure-region. hello kopiorna är i olika fel och uppgradera domäner (begrepp finns [detta] [ planning-guide-3.2] kapitlet i hello [Planeringshandboken][planning-guide]). En ny replik genereras automatiskt vid en replik av hello data gå ur funktion på grund av tooa lagring nodfel eller diskfel. 
* **GEO-Redundant lagring (GRS)**: I det här fallet är det en asynkron replikering som en ytterligare tre repliker av hello data i en annan Azure-Region som är i de flesta fall hello hello samma geografiska region (till exempel Norra Europa och Väst Europa). Detta resulterar i tre ytterligare repliker, så att det finns sex repliker i summan. En variant av det här är ett tillägg där hello data i hello geo-replikerade Azure-region kan användas för skrivskyddade (läsbehörighet Geo-Redundant).
* **Zonen Redundant lagring (ZRS)**: I det här fallet hello hello tre repliker av hello data finns kvar i samma Azure-Region. Enligt beskrivningen i [detta] [ planning-guide-3.1] kapitel i hello [Planeringshandboken] [ planning-guide] en Azure-region kan vara ett tal för datacenter i närheten. Hello gäller LRS skulle hello repliker distribueras över hello olika datacenter som gör en Azure-region.

Mer information hittar du [här][storage-redundancy].

> [!NOTE]
> För DBMS distributioner rekommenderas inte hello användningen av Geo-Redundant lagring
> 
> Azure Storage Geo-replikering är asynkron. Replikering av individuella diskar monterade tooa enda virtuell dator inte är synkroniserade i Lås steg. Det är därför inte lämpligt tooreplicate DBMS-filer som är fördelade över olika diskar eller distribueras mot en programvarubaserad RAID baserat på flera diskar. DBMS-programmet kräver att hello beständiga disklagring exakt är synkroniserad över olika LUN och underliggande diskar/axlar. DBMS programvaran använder olika metoder toosequence IO skriva aktiviteter och ett DBMS rapporterar att hello disklagring mål för hello replikering är skadad om dessa varierar även av några millisekunder. Om en verkligen vill ha en konfiguration med en-databas har sträckts ut över flera diskar georeplikerad måste därför sådana replikering toobe utförs med databasen innebär och funktioner. En bör inte förlita dig på Azure Storage-Geo-replikering tooperform jobbet. 
> 
> hello problemet är enklaste tooexplain med ett system som exempel. Anta att du har ett SAP-system som överförts till Azure, vilket har åtta diskar som innehåller datafiler i hello DBMS plus en disk som innehåller hello transaktionsloggfilen. Var och en av diskarna nio har data som skrivs toothem i en konsekvent metod enligt toohello DBMS, om hello data skrivs toohello data eller transaktionen loggfiler.
> 
> I ordning tooproperly geo-replikering hello data och bibehålla en konsekvent databasbild, hello innehåll för alla nio diskar skulle ha toobe georeplikerad i hello ordning hello i/o-åtgärder har utförts mot hello nio olika diskar. Azure Storage geo-replikering tillåter dock inte toodeclare beroenden mellan diskar. Detta innebär inte känna hello faktum att hello innehållet i dessa nio olika diskar andra relaterade tooeach och att hello dataändringar är konsekventa endast vid replikering i hello ordning hello i/o-åtgärder i Microsoft Azure Storage geo-replikering hände över alla hello nio diskar.
> 
> Förutom risken att hög att hello georeplikerad bilder i hello scenariot inte ger en konsekvent databasen avbildning, finns det också en prestandaförsämring som visas med geo-redundant lagring som kan kraftigt påverka prestanda. Sammanfattningsvis: Använd inte den här typen av lagring redundans för DBMS typen arbetsbelastningar.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Mappningen av virtuella hårddiskar till virtuella Azure-datorn tjänstkonton för lagring
Det här kapitlet gäller endast tooAzure Storage-konton. Hello begränsningar som nämns i detta kapitel gäller inte om du planerar toouse hanterade diskar. Mer information om hanterade diskar finns i kapitlet [hanterade diskar] [ dbms-guide-managed-disks] i den här handboken.

Ett Azure Storage-konto är en administrativ konstruktion utan också ett ämne begränsningar. Medan hello begränsningar beror på om vi pratar om ett Azure-Standard Storage-konto eller ett Azure Premium Storage-konto. hello exakt funktioner och begränsningar finns [här][storage-scalability-targets]

Så för Azure standardlagring är det viktigt toonote det finns en gräns på hello IOPS per lagringskonto (rad som innehåller 'Totala begära hastigheten' i [hello artikel][storage-scalability-targets]). Det finns också en inledande högst 100 Lagringskonton per Azure-prenumeration (från och med juli 2015). Därför rekommenderas toobalance IOPS för virtuella datorer mellan flera lagringskonton när du använder Azure standardlagring. Medan en enda virtuell dator använder vi ett lagringskonto om möjligt. Om vi pratar om DBMS-distributioner där varje virtuell Hårddisk som finns på Azure standardlagring kunde nå sin kvot bör du därför bara distribuera 30-40 virtuella hårddiskar per Azure Storage-konto som använder Azure standardlagring. På hello däremot om du använder Azure Premium-lagring och vill toostore stor databas volymer, kanske du bra som IOPS. Men en Azure Premium Storage-konto är sätt mer restriktiva i datavolym än en Azure-Standard Storage-konto. Därför kan du bara distribuera ett begränsat antal virtuella hårddiskar inom Azure Premium Storage-konto innan träffa hello volym begränsningen. I slutet av hello betrakta ett Azure Storage-konto som en ”virtuell SAN” som har begränsade möjligheter i IOPS och/eller kapaciteten. Därför hello aktiviteten finns kvar, som i lokala distributioner toodefine hello layout hello virtuella hårddiskar på hello olika SAP system över hello olika 'tänkt SAN-enheter, eller Azure Storage-konton.

För Azure standardlagring rekommenderas inte toopresent storage från olika lagringsplatser konton tooa enkel VM om möjligt.

När du använder hello DS eller GS-serien av virtuella Azure-datorer, är det möjligt toomount virtuella hårddiskar från Lagringskonton i Azure-Standard och Premium-Lagringskonton. Användningsfall som skrivning säkerhetskopieringar till standardlagring backas upp av virtuella hårddiskar och med DBMS data och loggfiler på Premium-lagring kommer toomind där sådan heterogen lagring kan utnyttjas. 

Baserat på kunddistributioner och tester cirka 30 too40 virtuella hårddiskar som innehåller data databasfilerna och loggfilerna kan etableras på ett enda Azure Standard Storage-konto med acceptabel prestanda. Som tidigare nämnts är hello begränsning av Azure Premium Storage-konto sannolikt toobe hello datakapacitet klarar och inte IOPS.

Som med SAN-enheter lokalt, delning kräver vissa övervakning i ordning tooeventually upptäcka flaskhalsar på ett Azure Storage-konto. hello Azure övervakning tillägget för SAP och hello Azure-portalen är verktyg som kan använda toodetect upptagen Azure Storage-konton som kan leverera något sämre i/o-prestanda.  Om den här situationen har identifierats bör toomove upptagen VMs tooanother Azure Storage-konto. Se toohello [Deployment Guide] [ deployment-guide] information om hur tooactivate hello SAP värd övervakningsfunktionerna.

En annan artikel sammanfattning metodtips runt Azure standardlagring och Azure Storage-standardkonton hittar du här <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Hanterade diskar
Hanterade diskar är en ny resurstyp i Azure Resource Manager som kan användas i stället för virtuella hårddiskar som lagras i Azure Storage-konton. Hanterade diskar justeras automatiskt med hello Tillgänglighetsuppsättning hello virtuell dator som de är anslutna tooand därför öka hello tillgängligheten för virtuella datorer och hello-tjänster som körs på hello virtuella datorn. toolearn läsa fler hello [översiktsartikel](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

SAP stöder för närvarande endast hanteras Premiumdiskar. Läs SAP-kommentar [1928533] för mer information.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-tooazure-premium-storage"></a>Flytta distribueras DBMS virtuella datorer från Azure standardlagring tooAzure Premium-lagring
Vi får ganska vissa scenarier där du som kund vill toomove en distribuerad virtuell dator från Azure standardlagring till Azure Premium-lagring. Om diskarna lagras i Azure Storage-konton kan är det inte möjligt utan att fysiskt flytta hello data. Det finns flera sätt tooachieve hello målet:

* Du kan bara kopiera alla virtuella hårddiskar, bas-VHD som virtuella hårddiskar data till en ny Azure Premium Storage-konto. Ofta du har valt hello antalet virtuella hårddiskar i Azure standardlagring inte på grund av hello fakta du behövs hello datavolym. Dock du så många virtuella hårddiskar på grund av hello IOPS. Nu när du flyttar tooAzure hello Premium-lagring som du kan gå sätt färre virtuella hårddiskar tooachieve samma IOPS genomflöde. Angivna hello faktum att Azure standardlagring du betalar för hello används för data och inte hello nominell diskens storlek hello antalet virtuella hårddiskar ingen verkligen roll vad gäller kostnader. Med Azure Premium-lagring, skulle du dock betalar för hello nominell diskstorleken. De flesta kunder hello försök därför tookeep hello antal Azure virtuella hårddiskar i Premium-lagring på hello antalet nödvändiga tooachieve hello IOPS genomströmning behövs. Därför Bestäm de flesta kunder mot hello sätt på ett enkelt 1:1 kopia.
* Om du ännu inte monterade montera en enda virtuell Hårddisk som kan innehålla en säkerhetskopia av databasen för din SAP-databas. Hello säkerhetskopieringen du demontera inklusive hello VHD som innehåller hello säkerhetskopiering på alla virtuella hårddiskar och kopiera hello bas-VHD och hello VHD med hello säkerhetskopiering till Azure Premium Storage-konto. Du skulle distribuera hello VM baserat på hello base VHD- och montera hello VHD med hello säkerhetskopiering. Nu kan du skapa ytterligare tom Premiumdiskar med lagringsutrymme för hello VM som används toorestore hello-databas till. Detta förutsätter att hello DBMS kan du toochange sökvägar toohello data och loggfilen filer som en del av hello återställningsprocessen.
* En annan möjlighet är en variation av hello tidigare processen, där du bara kopiera hello säkerhetskopiering VHD till Azure Premium-lagring och bifoga den mot en virtuell dator som du nyligen har distribuerats och installerats.
* hello fjärde möjligheten väljer du när du är behöva toochange hello antalet datafiler i databasen. I så fall skulle du utföra en SAP homogen kopian med hjälp av export/import. Placera de exportera filerna till en virtuell Hårddisk som har kopierats till Azure Premium Storage-konto och koppla den tooa VM som du använder toorun hello importera processer. Kunder använda denna möjlighet huvudsakligen när de vill toodecrease hello antalet datafiler.

Om du använder hanterade diskar kan du migrera tooPremium lagring av:

1. Frigör hello virtuell dator
2. Om det behövs, ändra storlek på hello tooa storlek för virtuell dator som har stöd för Premium-lagring (till exempel DS eller GS)
3. Ändra hello hanteras Disk konto typ tooPremium (SSD)
4. Starta den virtuella datorn

### <a name="deployment-of-vms-for-sap-in-azure"></a>Distribution av virtuella datorer för SAP i Azure
Microsoft Azure tillhandahåller flera olika sätt toodeploy virtuella datorer och associerade diskar. Vilket är viktigt toounderstand hello skillnader, eftersom förberedelser hello virtuella skilja sig åt beroende på hello sätt för distribution. I allmänhet titta vi i hello-scenarier som beskrivs i följande kapitlen hello.

#### <a name="deploying-a-vm-from-hello-azure-marketplace"></a>Distribuera en virtuell dator från hello Azure Marketplace
Du vill tootake en Microsoft eller tredje part angivna avbildningen från hello Azure Marketplace toodeploy den virtuella datorn. När du har distribuerat den virtuella datorn i Azure måste du följa hello samma riktlinjer och verktyg tooinstall hello SAP-program i din virtuella dator som du gör i en lokal miljö. För att installera hello SAP-program i hello Azure VM, SAP och Microsoft rekommenderar överföring och lagra hello SAP-installationsmediet i diskar eller toocreate en Azure VM fungerar som en 'server', som innehåller alla hello-installationsmediet för nödvändiga SAP.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Distribuera en virtuell dator med en kundspecifika generaliserad avbildning
På grund av krav för toospecific om ditt operativsystem eller DBMS-version, kan hello tillhandahålls bilder i hello Azure Marketplace inte passar dina behov. Därför kanske du måste toocreate en VM som använder en egen ”privat” OS/DBMS VM-avbildning som kan distribueras efteråt flera gånger. tooprepare en ”privat” avbildning för duplicering hello OS måste vara generaliserad på hello lokalt VM. Se toohello [Deployment Guide] [ deployment-guide] mer information om hur toogeneralize en virtuell dator.

Om du redan har installerat SAP innehållet i den lokala virtuella datorn (särskilt för nivå 2-system), kan du anpassa hello SAP systeminställningar när hello distribution av hello Azure VM via hello förekomst proceduren som stöds av hello SAP tillhandahållande av programvara Manager (SAP-kommentar [1619720]). Annars kan du installera program hello senare efter hello distributionen av hello Azure VM.

Från och med hello databasen innehåll som används av hello SAP-programmet, du kan generera hello innehållet nyligen av en SAP-installation eller du kan importera ditt innehåll till Azure med hjälp av en virtuell Hårddisk med en säkerhetskopia av databasen DBMS eller genom att utnyttja funktionerna i hello DBMS toodirectly säkerhetskopiering i Microsoft Azure Storage. I det här fallet kan du även förbereda virtuella hårddiskar med hello DBMS data logga filer på lokala och sedan importera dem som diskar till Azure. Men hello överföringen av DBMS-data som läses in från lokala tooAzure fungerar över VHD-diskar som behöver toobe förberedd lokalt.

#### <a name="moving-a-vm-from-on-premises-tooazure-with-a-non-generalized-disk"></a>Flytta en virtuell dator från lokal tooAzure med en icke-generaliserad disk
Du planerar toomove ett särskilt SAP-system från lokala tooAzure (lift och SKIFT). Detta kan göras genom att överföra hello disken som innehåller hello OS, hello SAP binärfiler och eventuell DBMS-binärfiler plus hello diskar med hello data och loggfiler för hello DBMS tooAzure. I motsatt tooscenario #2 ovan behålla du hello värdnamn, SAP-SID och SAP användarkonton i hello Azure VM som de har konfigurerats i hello lokala miljö. Därför är att generalisera hello bilden inte nödvändigt. Det här fallet gäller främst för anslutningar mellan lokala scenarier där en del av hello SAP liggande kör lokalt och delar för på Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Hög tillgänglighet och katastrofåterställning med virtuella Azure-datorer
Azure erbjuder hello följande hög tillgänglighet och Disaster Recovery (DR) funktioner som gäller toodifferent komponenter som vi använder för SAP och DBMS-distributioner

### <a name="vms-deployed-on-azure-nodes"></a>Virtuella datorer som distribueras på Azure-noder
hello Azure-plattformen erbjuder inte funktioner, till exempel Direktmigrering för distribuerade virtuella datorer. Det innebär att om det krävs Underhåll på ett serverkluster som är distribuerad på en virtuell dator, hello VM måste tooget stoppas och startas om. Underhåll i Azure utförs med så kallade uppgradera domäner inom kluster med servrar. Endast en uppgradera domän i taget bevaras. Det finns ett avbrott i tjänsten när hello VM stängs av under en omstart, och underhåll utförs virtuella datorn startas om. De flesta DBMS-leverantörer men ger hög tillgänglighet och katastrofåterställning funktioner som snabbt startar om hello DBMS-tjänster på en annan nod om hello primära noden är tillgänglig. hello Azure-plattformen erbjuder funktionalitet toodistribute virtuella datorer, lagring och andra Azure-tjänster över uppgradera domäner tooensure som planerat underhåll eller infrastruktur fel påverkar endast en liten del av virtuella datorer eller tjänster.  Med noggrann planering, är det möjligt tooachieve tillgänglighet nivåer jämförbara tooon lokal infrastruktur.

Microsoft Azure-Tillgänglighetsuppsättningar är en logisk gruppering av virtuella datorer eller tjänster som säkerställer att virtuella datorer och andra tjänster som är distribuerade toodifferent fel och uppgradera domäner inom ett kluster så att skulle det bara finnas en nodavstängning på någon punkt i tiden (läsa [(Linux)] [ virtual-machines-manage-availability-linux] eller [(Windows)] [ virtual-machines-manage-availability-windows] mer information).

Den måste toobe som konfigurerats av syfte vid distribution av virtuella datorer som visas här:

![Definition av Tillgänglighetsuppsättning för DBMS HA konfigurationer][dbms-guide-figure-200]

Om vi vill toocreate högtillgängliga konfigurationer för DBMS-distributioner (oberoende av hello enskilda DBMS HA funktioner används) måste hello DBMS virtuella datorer du:

* Lägg till hello VMs toohello samma Azure Virtual Network (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* hello virtuella datorer i hello konfiguration för hög tillgänglighet bör också vara i hello samma undernät. Det går inte att namnmatchningen hello olika undernät i distributioner för endast molnbaserad, IP-upplösning fungerar bara. Med hjälp av plats-till-plats eller ExpressRoute-anslutning för anslutningar mellan lokala distributioner, redan ett nätverk med minst ett undernät. Namnmatchning utförs enligt toohello lokala AD-principer och nätverksinfrastruktur. 

[comment]: <> (MSSedusch TODO testa om det fortfarande true i ARM)

#### <a name="ip-addresses"></a>IP-adresser
Det rekommenderas starkt toosetup hello virtuella datorer för hög tillgänglighet konfigurationer på ett flexibelt sätt. Förlita dig på IP-adresser tooaddress hello HA samarbetspartners inom hello konfiguration för hög tillgänglighet är inte tillförlitligt i Azure om statiska IP-adresser som används. Det finns två ”Stäng” koncept i Azure:

* Stänga ned via Azure-portalen eller Azure PowerShell-cmdlet Stop-AzureRmVM: I det här fallet hello virtuella hämtar avstängning och frigör tilldelas. Azure-konto debiteras inte längre för den här virtuella datorn så att hello endast avgifter innebära för hello lagringsutrymme som används. Om hello privata IP-adressen för nätverksgränssnittet hello inte var statiska hello IP-adressen frisläpps och garanteras inte nätverksgränssnittet hello hämtar hello gamla IP-adressen igen efter en omstart av hello VM. Utför hello stänga via hello Azure-portalen eller genom att anropa stoppa AzureRmVM automatiskt medför inaktiveringen allokering. Om du inte vill toodeallocate hello datorn använda stoppa AzureRmVM - StayProvisioned 
* Om du stänger av hello virtuell dator från en OS-nivå, hämtar hello VM stängs av och inte Frigör allokeras. Men i det här fallet debiteras ditt Azure-konto fortfarande för hello VM, trots hello att den stängs. I sådana fall stoppats hello tilldelning av hello IP-adress tooa VM förblir intakta. Stänger av hello tvingar virtuell dator från inom automatiskt inte inaktiveringen allokering.

Även för anslutningar mellan lokala scenarier som standard innebär en avstängning och avinstallation tilldelning inaktiveringen tilldelning av hello IP-adresser från hello VM, även om lokala principer i DHCP-inställningarna är olika. 

* hello undantag är om en tilldelas en statisk IP-adress tooa nätverksgränssnittet som beskrivs [här][virtual-networks-reserved-private-ip].
* I sådana fall hello IP-adress ligger kvar så länge hello nätverksgränssnittet inte tas bort.

> [!IMPORTANT]
> Hello Rensa rekommendation är toosetup hello virtuella datorer som samarbetar med i en konfiguration för DBMS hög tillgänglighet och Katastrofåterställning i Azure så att det finns en fungerande namnmatchning mellan hello olika virtuella datorer som är involverad i ordning tookeep hello hela distributionen enkel och hanterbar.
> 
> 

## <a name="deployment-of-host-monitoring"></a>Distribution av värden övervakning
För effektiv användning av SAP-program i Azure Virtual Machines kräver SAP hello möjlighet tooget värden övervakningsdata från hello fysiska värdar som kör hello Azure virtuella datorer. En specifik SAP Värdagenten korrigeringsnivå krävs som gör att den här funktionen i SAPOSCOL och SAP värden Agent. hello exakt korrigeringsnivå dokumenteras i SAP-kommentar [1409604].

Hello information om distribution av komponenter som levererar värden data tooSAPOSCOL och SAP Värdagenten och hello livscykeln för hantering av dessa komponenter finns i toohello [Deployment Guide][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Närmare information tooMicrosoft SQL Server
### <a name="sql-server-iaas"></a>SQL Server IaaS
Från och med Microsoft Azure kan migrera du enkelt dina befintliga SQL Server-program som bygger på Windows Server-plattformen tooAzure virtuella datorer. SQL Server på en virtuell dator kan du tooreduce hello totala ägandekostnaden för distribution, hantering och underhåll av breda företagsprogram genom att enkelt migrera dessa program tooMicrosoft Azure. Med SQL Server i en virtuell dator i Azure, kan administratörer och utvecklare fortfarande använda hello samma utvecklings- och administrationskostnader verktyg som är tillgängliga på lokalt. 

> [!IMPORTANT]
> Vi diskuterar inte Microsoft Azure SQL Database, vilket är en plattform som en tjänsterbjudande av hello Microsoft Azure-plattformen. hello diskussion i det här dokumentet handlar om hello SQL Server-produkt som körs som det är känt för lokala distributioner i Azure Virtual Machines, utnyttja hello infrastruktur som en tjänst-funktion i Azure. Databasen funktioner och funktionalitet mellan dessa två erbjudanden är olika och bör inte blandas med varandra. Se även: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Vi rekommenderar starkt tooreview [detta] [ virtual-machines-sql-server-infrastructure-services] dokumentationen innan du fortsätter.

I hello samman och nämns följande avsnitt delar av delar av hello dokumentationen under hello länken ovan. Närmare information kring SAP nämns samt och några koncept som beskrivs i detalj. Men rekommenderas det starkt toowork via hello dokumentationen ovan första innan du läser specifika hello SQL Server-dokumentationen.

Det finns några SQL-Server i IaaS specifik information som du bör känna till innan du fortsätter:

* **Virtual Machine SLA**: det finns ett SLA för virtuella datorer som körs i Azure som finns här: <https://azure.microsoft.com/support/legal/sla/>  
* **Stöd för SQL-Version**: för SAP-kunder vi stöder SQL Server 2008 R2 och högre på Microsoft Azure-dator. Tidigare versioner stöds inte. Granska övergripande [Support Statement](https://support.microsoft.com/kb/956893) för mer information. Observera att i allmänhet SQL Server 2008 stöds av Microsoft samt. Men på grund av toosignificant funktioner för SAP, som introducerades i SQL Server 2008 R2, SQL Server 2008 R2 är hello lägsta version för SAP. Tänk på att SQL Server 2012 och 2014 har utökats med djupare integrering hello IaaS-scenario (till exempel säkerhetskopiera direkt mot Azure Storage). Därför begränsar vi det här dokumentet tooSQL Server 2012 och 2014 med den senaste korrigeringsnivå för Azure.
* **Stöd för SQL-funktionen**: mest SQL Server-funktioner stöds på Microsoft Azure-datorer med vissa undantag. **SQL Server Failover Clustering med hjälp av delade diskar stöds inte**.  Distribuerade tekniker som databasspegling, AlwaysOn Availability Groups, replikering, Loggöverföring och Service Broker stöds i en enda Azure-Region. SQL Server AlwaysOn också stöds mellan olika Azure-regioner som dokumenteras här: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Granska hello [Support Statement](https://support.microsoft.com/kb/956893) för mer information. Ett exempel på hur toodeploy AlwaysOn-konfigurationen visas i [detta] [ virtual-machines-workload-template-sql-alwayson] artikel. Dessutom kolla hello metodtips dokumenterade [här][virtual-machines-sql-server-infrastructure-services] 
* **Prestanda för SQL**: vi är säker på att Microsoft Azure virtuella värddatorerna utför mycket bra i virtualiseringslösningar för jämförelse tooother offentliga moln, men enskilda resultaten kan variera. Checka ut [detta] [ virtual-machines-sql-server-performance-best-practices] artikel.
* **Med hjälp av avbildningar från Azure Marketplace**: hello snabbaste sättet toodeploy en ny Microsoft Azure VM är toouse en bild från hello Azure Marketplace. Det finns avbildningar i hello Azure Marketplace, som innehåller SQL Server. hello bilder där SQL Server redan är installerad kan inte användas direkt för SAP NetWeaver program. hello beror hello standardsortering för SQL Server är installerade på dessa avbildningar och inte hello sorteringen krävs av SAP NetWeaver system. I order toouse dessa bilder, kontrollera hello stegen i kapitel [med hjälp av en SQL Server-avbildning utanför hello Microsoft Azure Marketplace][dbms-guide-5.6]. 
* Checka ut [prisinformation](https://azure.microsoft.com/pricing/) för mer information. Hej [SQL Server 2012 Licensing Guide](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) och [SQL Server 2014 licensiering Guide](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) är också en viktig resurs.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Riktlinjer för SQL Server-konfiguration för SAP-relaterade SQL Server-installationerna i virtuella Azure-datorer
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Rekommendationer för VM/VHD-strukturen för SAP-relaterade SQL Server-distributioner
I enlighet med hello allmän beskrivning och körbara filer i SQL Server belägen eller installerats i hello systemenhet hello VM OS-disken (enhet C:\).  De flesta av hello systemdatabaser för SQL Server används vanligtvis inte på en hög nivå av SAP NetWeaver arbetsbelastning. Därför kan hello systemdatabaser av SQL Server (master, msdb och modell) finns kvar på hello C:\-enhet. Ett undantag kunde tempdb som hello gäller vissa SAP ERP och alla BW arbetsbelastningar kan kräva högre datavolym eller i/o operations volymen, som inte får plats i hello ursprungliga virtuella datorn. För sådana system ska hello följande steg utföras:

* Flytta hello primära tempdb data fil(er) toohello samma logiska enhet som hello primära datafiler av hello SAP-databasen.
* Lägg till eventuella ytterligare tempdb data filer tooeach av hello andra logiska enheter som innehåller en datafil av hello SAP-användardatabasen.
* Lägg till hello tempdb logfile toohello logisk enhet, som innehåller loggfil hello användardatabas.
* **Endast för VM-typer som använder lokala SSD** på hello compute-nod tempdb data och loggfilen filer kan placeras på hello D:\ enhet. Dock kan det vara rekommenderas toouse flera tempdb-datafiler. Tänk på D:\ enhetsvolymer är olika baserat på hello VM-typen.

De här konfigurationerna aktivera tempdb tooconsume mer utrymme än hello systemenhet är kan tooprovide. I ordning toodetermine hello rätt tempdb storlek kan en Kontrollera hello tempdb storlekar på befintliga system som kör lokalt. En sådan konfiguration skulle dessutom aktivera IOPS siffror mot tempdb som kan tillhandahållas med hello systemenheten. System som kör lokalt kan det är används toomonitor i/o-arbetsbelastning mot tempdb så att du kan härleda hello IOPS siffror som du förväntar dig toosee på tempdb.

En VM-konfiguration, som kör SQL Server med en SAP-databas och där tempdb data och tempdb logfile placeras på hello D:\ enhet ser ut som:

![För referenskonfiguration av Azure IaaS-VM för SAP][dbms-guide-figure-300]

Tänk på att hello D:\ enhet har olika storlekar som är beroende av hello VM-typen. Beroende på hello kravet på tempdb kan det vara framtvingad toopair tempdb data och loggfiler med hello SAP-data från databasen och loggfiler i fall där D:\ enheten är för liten.

#### <a name="formatting-hello-disks"></a>Formatering hello diskar
Filer bör vara 64 kB för SQL Server hello NTFS blockstorleken för diskar som innehåller SQL Server-data och loggfilen. Det finns inget behov av tooformat hello D:\ enhet. Den här enheten kommer före formaterad.

I ordning toomake till att hello återställning eller skapandet av databaser inte initierar hello datafiler av nollställning hello innehållet i hello filer, en bör se till har att hello användaren kontexten hello SQL Server-tjänsten körs i en viss behörighet. Användare i administratörsgruppen för hello Windows har vanligtvis dessa behörigheter. Om hello SQL Server-tjänsten körs i hello användarkontext med icke - Windows administratör, måste tooassign som användaren hello användarrättigheten 'Utföra underhållsaktiviteter'.  Hello mer information finns i det här Microsoft Knowledge Base-artikel: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Effekten av databasen komprimering
Alla åtgärder, vilket minskar IOPS kan hjälpa toostretch hello arbetsbelastningen kan köra i ett IaaS-scenario som Azure i konfigurationer där i/o-bandbredd kan bli en begränsande faktor. Därför om inte har gjort rekommenderas tillämpa SQL Server-sidan komprimering av både SAP och Microsoft innan du laddar upp en befintlig tooAzure för SAP-databasen.

hello rekommendation tooperform komprimering av databasen innan du laddar upp tooAzure får slut på två skäl:

* hello mängden data toobe som överförts är lägre.
* hello varaktighet för hello komprimering körningen är kortare förutsatt att något kan använda starkare maskinvara med flera processorer eller högre i/o-bandbredd eller mindre i/o-svarstid lokalt.
* Databasen är mindre leda tooless kostnader för diskallokering

Databasen komprimering fungerar också i ett Azure Virtual Machines som lokalt. Mer information om hur toocompress en befintlig SAP SQL Server-databas finns här: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014--storing-database-files-directly-on-azure-blob-storage"></a>SQLServer 2014 – lagra databasfilerna direkt på Azure Blob Storage
SQL Server 2014 öppnas hello möjligheten toostore databasfilerna direkt på Azure Blob Store utan hello 'wrapper' av en virtuell Hårddisk runtom. Särskilt med hjälp av Azure standardlagring eller mindre VM-typer kan detta scenarier där du kan lösa hello gränserna för IOPS skulle tillämpas av ett begränsat antal diskar som kan vara monterade toosome mindre VM-typer. Detta fungerar för användardatabaser men inte för systemdatabaser av SQL Server. Den fungerar även för data och loggfiler för SQL Server. Om du vill att toodeploy en SAP SQL Server-databas det här sättet i stället för ”radbryts' till virtuella hårddiskar, Tänk hello följande:

* hello Storage-konto som används måste toobe i hello samma Azure-Region som hello ett som är används toodeploy hello Virtuella SQL Server körs i.
* Överväganden avseende som angavs tidigare hello distribution av virtuella hårddiskar över olika Azure Storage-konton gäller för den här metoden samt distributioner. Sätt hello i/o-åtgärder antal mot hello gränserna för hello Azure Storage-konto.

[comment]: <> (MSSedusch TODO men det här använder nätverket och inte lagring bandbredd, inte den?)

Information om den här typen av distribution visas här: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

I ordning toostore SQL Server-datafiler direkt på Azure Premium-lagring, behöver du toohave en minsta SQL Server 2014 korrigering version som dokumenteras här: <https://support.microsoft.com/kb/3063054>. Lagra filer för SQL Server-data på Azure standardlagring fungerar med hello släppt version av SQL Server 2014. Hello samma korrigeringsfiler innehåller dock en annan serie korrigeringar som gör det mer tillförlitlig hello direkt användning av Azure Blob Storage för SQL Server-datafiler och säkerhetskopieringar. Därför bör du använda dessa korrigeringar i allmänhet.

### <a name="sql-server-2014-buffer-pool-extension"></a>Buffertpooltillägget för SQL Server 2014
SQL Server 2014 introducerade en ny funktion som kallas Buffertpooltillägget. Den här funktionen utökar hello buffertpool av SQL Server som är kvar i minnet med en andra nivå cache som backas upp av lokala SSD-enheter på en server eller virtuell dator. Detta gör att tookeep en större arbetsminnet för data ”i minnet'. Jämfört med tooaccessing Azure standardlagring hello åtkomst till hello-tillägget för hello buffertpool som är lagrad på lokala SSD av en Azure VM är snabbare många faktorer.  Därför kan utnyttja hello D:\ lokalt hello VM typer som har utmärkt IOPS och dataflöde vara en mycket rimligt sätt tooreduce hello IOPS ladda mot Azure Storage och kraftigt förbättra svarstiden för frågor. Detta gäller särskilt om du inte använder Premium-lagring. Premium-lagring och hello användning av hello Premium Azure Läs-Cache på hello beräkningsnod förväntas några betydande skillnader som rekommenderas för datafiler. Orsaken är att båda cacheminnen (Buffertpooltillägget för SQL Server- och Premium-lagring Läs-Cache) använder hello lokala diskar på datornoderna hello.
Mer information om den här funktionen finns i den här dokumentationen: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Säkerhetskopiering/återställning överväganden för SQL Server
Vid distribution av SQL Server i Azure måste din backup metod granskas. Även om hello system inte är en produktiv system måste hello SAP-databasen med SQL Server som värd säkerhetskopieras regelbundet. Eftersom Azure Storage håller tre bilder, är nu en säkerhetskopia mindre viktiga avseende toocompensating en krasch lagring. hello beror prioritet för att bibehålla en korrekt plan för säkerhetskopiering och återställning på mer om att du kan kompensera för logiska/manuell fel genom att tillhandahålla punkt i tiden återställningsfunktioner. Så hello målet är tooeither använda säkerhetskopior toorestore hello databas tillbaka tooa vissa peka i tid eller toouse hello säkerhetskopieringar i Azure tooseed ett annat system genom att kopiera hello befintlig databas. T.ex, du kan överföra från en 2-skikts SAP tooa 3-skikts system konfigurationsinställningar för hello samma system genom att återställa en säkerhetskopia.

Det finns tre olika sätt toobackup SQL Server-tooAzure lagring:

1. SQL Server 2012 CU4 och högre kan internt säkerhetskopierade databaser tooa URL. Detta beskrivs i hello blogg [nya funktioner i SQL Server 2014 – del 5 – säkerhetskopiering/återställning förbättringar](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Se kapitel [SQL Server 2012 SP1 CU4 och senare][dbms-guide-5.5.1].
2. SQL Server versioner tidigare tooSQL 2012 CU4 kan använda en omdirigering funktioner toobackup tooa VHD och i princip flytta hello skrivdataström mot en Azure-lagringsplats som har konfigurerats. Se kapitel [SQL Server 2012 SP1 CU3 och tidigare versioner][dbms-guide-5.5.2].
3. hello sista metoden är tooperform ett kommando för säkerhetskopiering toodisk av konventionella SQL Server på en diskenhet. Detta är identiska toohello lokala distributionen mönster som beskrivs inte i detalj i detta dokument.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 och senare
Den här funktionen tillåter toodirectly säkerhetskopiering tooAzure BLOB storage. Utan den här metoden måste du säkerhetskopiera tooother diskar som ska använda disk- och IOPS kapacitet. hello idé är i princip:

 ![Med hjälp av SQL Server 2012 Backup tooMicrosoft Azure Storage BLOB][dbms-guide-figure-400]

hello fördelen är i det här fallet att något inte behöver toospend diskar toostore SQL Server säkerhetskopieras på. Så du har färre diskar som är allokerade och hello hela bandbredden för disk-IOPS kan användas för data och loggfiler. Observera att hello maximal storlek för en säkerhetskopia är begränsad tooa högst 1 TB enligt beskrivningen i avsnittet hello-begränsningar ”i den här artikeln: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations>. Hej funktioner som beskrivs i kapitlet om hello säkerhetskopieringsstorlek trots med SQL Server-säkerhetskopiering komprimering skulle överskrida 1 TB i storlek, [SQL Server 2012 SP1 CU3 och tidigare versioner] [ dbms-guide-5.5.2] i det här dokumentet toobe används.

[Relaterad dokumentation](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) som beskriver hello återställning av databaser från säkerhetskopior mot Azure Blob Store rekommenderar inte toorestore direkt från Azure BLOB store om hello säkerhetskopia > 25 GB. hello rekommendation i den här artikeln är bara baserat på prestandaöverväganden och inte på grund av toofunctional begränsningar. Därför kan olika villkor tillämpas på en fall till fall.

Dokumentation om hur den här typen av säkerhetskopiering har konfigurerats och utnyttjas kan hittas i [detta](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) självstudiekursen

Ett exempel på hello ordningsföljden kan läsas [här](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Automatiserad säkerhetskopiering, är det av högsta vikt toomake att hello BLOB för varje säkerhetskopiering namnges på olika sätt. Annars de skrivs över och hello restore-kedjan har brutits.

Inte toomix in saker mellan hello tre olika typer av säkerhetskopior är, det lämpligt toocreate olika behållare under hello storage-konto som används för säkerhetskopieringen. hello behållare kan vara av virtuell dator bara eller efter typ av virtuell dator och säkerhetskopiering. hello-schemat kan se ut så:

 ![Med hjälp av SQL Server 2012 Backup tooMicrosoft Azure Storage BLOB – olika behållare under separat Storage-konto][dbms-guide-figure-500]

Hello exemplet ovan, hello säkerhetskopieringar inte kommer att utföras i samma lagringsutrymme konto där hello hello distribuerade virtuella datorer. Det är ett nytt lagringskonto för hello säkerhetskopieringar. Inom hello storage-konton, skulle det finnas olika behållare som skapats med en matris med hello typ av säkerhetskopiering och hello VM-namn. Sådana segmentering gör det enklare tooadministrate hello säkerhetskopior av hello olika virtuella datorer.

hello blob som en direkt skriver hello säkerhetskopieringar till, läggs inte toohello antal hello datadiskar av en virtuell dator. Därför en kan maximera hello högst datadiskar som monterats av hello specifika VM SKU för hello data och transaktionen loggfilen och fortfarande köra en säkerhetskopiering mot en lagringsbehållare. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 och tidigare versioner
hello första steg i ordning tooachieve måste du utföra en säkerhetskopiering direkt mot Azure Storage är toodownload hello msi, som är länkad för[detta](https://www.microsoft.com/download/details.aspx?id=40740) KBA artikel.

Hämta installationsfilen för hello x64 och hello-dokumentationen. hello filen installerar ett program som heter: ”Microsoft SQL Server-säkerhetskopiering tooMicrosoft Azure verktyget'. Dokumentationen hello hello produkten noggrant.  hello verktyget fungerar i praktiken hello följande sätt:

* Från hello SQL Server-side, definieras en plats för säkerhetskopiering av SQL Server hello (Använd inte hello D:\ enhet för den här).
* hello-verktyget kan du toodefine regler som kan vara används toodirect olika typer av säkerhetskopior toodifferent Azure Storage-behållare.
* När hello regler är på plats, omdirigerar hello verktyget hello skrivdataström av hello säkerhetskopiering tooone av hello VHD-diskar toohello Azure Storage-plats, som definierades tidigare.
* hello verktyget lämnar en liten stub-fil med några KB storleken på hello VHD/disken, som har definierats för hello SQL Server säkerhetskopiering. **Den här filen ska lämnas på hello lagringsplats eftersom den är obligatorisk toorestore igen från Azure Storage.**
  * Om du har tappat bort hello stub-fil (till exempel genom förlust av hello lagringsmedia som innehöll hello stub-fil) och du har valt alternativet hello säkerhetskopiera tooa Microsoft Azure Storage-konto, kan du återställa hello stub-filen via Microsoft Azure Storage genom att Hämta det från hello lagringsbehållaren den släpptes. Du bör placera hello stub-filen till en mapp på den lokala datorn för hello där hello verktyget är konfigurerade toodetect och ladda upp toohello behållare med hello samma Krypteringslösenord om kryptering används med hello ursprungliga regeln. 

Det innebär att hello schemat som beskrivs ovan för nyare versioner av SQL Server kan du försätta samt för SQL Server-versioner som inte tillåter att direkt adressen en Azure-lagringsplats.

Den här metoden ska inte användas med nyare SQL Server-versioner som stöder stödjande in internt mot Azure Storage. Undantag är där begränsningar av hello interna säkerhetskopiering till Azure blockerar interna säkerhetskopiering körning i Azure.

#### <a name="other-possibilities-toobackup-sql-server-databases"></a>Andra möjligheterna toobackup SQL Server-databaser
Andra möjligheterna toobackup databaser är tooattach ytterligare data diskar tooa VM som du använder toostore säkerhetskopieringar på. I så fall behöver du toomake till att hello diskar inte kör fullständig. Om så är fallet hello måste toounmount hello diskar och så toospeak 'Arkivera' den och Ersätt den med en ny tom disk. Om du går ned den sökvägen du tookeep dessa virtuella hårddiskar i separata Azure Storage-konton från hello som hello virtuella hårddiskar med hello-databasfiler.

En andra alternativet är toouse en stor virtuell dator som kan ha många diskar som är anslutna, till exempel en D14 med 32VHDs. Använd lagringsutrymmen toobuild en flexibel miljö där du kan skapa resurser som används sedan som mål för säkerhetskopiering för hello olika DBMS-servrar.

Metodtips fick dokumenterade [här](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) samt. 

#### <a name="performance-considerations-for-backupsrestores"></a>Prestandaöverväganden för säkerhetskopiering/återställning
Säkerhetskopiering/återställning prestanda är beroende av hur många volymer kan läsas parallellt och vilka hello genomströmning av volymerna kanske som bare metal-distributioner. Hello processorförbrukningen som används av säkerhetskopieringskomprimering kan dessutom spela en viktig roll på virtuella datorer med bara in tooeight CPU-trådar. Därför kan anta en:

* hello färre hello antalet diskar används toostore hello-datafiler, hello mindre hello totala genomflödet i läsning.
* Hej mindre hello antalet CPU-trådar i hello VM, hello allvarligare hello effekten av säkerhetskopieringskomprimering.
* Hej färre mål (BLOB, virtuella hårddiskar eller diskar) toowrite Hej säkerhetskopiering till, hello mindre hello genomflöde.
* hello mindre hello VM-storlek, hello mindre hello genomströmning lagringskvoten skrivning och läsning från Azure Storage. Oberoende av om hello säkerhetskopior lagras direkt på Azure Blob eller om de lagras i virtuella hårddiskar som är lagrade i Azure BLOB igen.

När du använder Microsoft Azure Storage BLOB som hello mål för säkerhetskopian i senare versioner, är begränsade toodesignating endast ett URL-mål för varje specifik säkerhetskopiering.

Men när du använder hello ”Microsoft SQL Server-säkerhetskopiering tooMicrosoft Azure verktyget' i äldre versioner, kan du definiera mer än en målfil. Hello säkerhetskopiering kan skala med fler än ett mål och hello genomflödet i hello säkerhetskopiering är högre. Detta leder sedan flera filer samt i hello Azure Storage-konto. I våra tester med flera filen mål en definitivt kan uppnå hello dataflöde, vilket kan uppnå med hello säkerhetskopiering tillägg genomföras från SQL Server 2012 SP1 CU4 på. Du också blockeras inte av hello 1TB gränsen som hello interna säkerhetskopiering till Azure.

Tänk dock på, hello genomströmning också är beroende av hello platsen för hello Azure Storage-konto du använder för hello säkerhetskopieringen. En idé kanske toolocate hello storage-konto i en annan region än hello virtuella datorer som körs i. Du skulle till exempel köra hello VM-konfiguration i västra Europa men placera hello Storage-konto som du använder tooback upp mot i Norra Europa. Som verkligen påverkar hello säkerhetskopiering genomflöde och är inte troligt toogenerate en genomströmning på 150MB per sekund som det är möjligt i fall där hello mål-lagringskontot och hello virtuella datorer körs i hello toobe samma regionala datacenter.

#### <a name="managing-backup-blobs"></a>Hanterar säkerhetskopierade BLOB
Det finns ett krav toomanage hello säkerhetskopieringar på egen hand. Eftersom hello förväntan att många blobbar skapas genom att köra säkerhetskopieringar av transaktionsloggen ofta, kan administration av dessa blobbar enkelt överbelasta hello Azure-portalen. Därför är det recommendable tooleverage en Azure Lagringsutforskaren. Det finns flera bra de tillgängliga, vilket hjälper toomanage ett Azure storage-konto

* Microsoft Visual Studio med Azure SDK är installerat (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure Lagringsutforskaren (<https://azure.microsoft.com/downloads/>)
* Verktyg från tredje part

En mer detaljerad information om säkerhetskopiering och SAP på Azure finns för[hello SAP säkerhetskopiering guiden](sap-hana-backup-guide.md) för mer information.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Med hjälp av en SQL Server-avbildning utanför hello Microsoft Azure Marketplace
Microsoft erbjuder virtuella datorer i hello Azure Marketplace, som redan innehåller versioner av SQL Server. För SAP-kunder som kräver licenser för SQL Server och Windows, kan det vara en möjlighet toobasically omfattar hello behovet av licenser för snurrande upp virtuella datorer med SQL Server redan har installerats. Ordning toouse dessa avbildningar för SAP, hello följande överväganden måste toobe görs:

* hello SQL Server icke-utvärderingsversioner hämta högre kostnader än bara en 'Endast för Windows ”virtuell dator distribueras från Azure Marketplace. Se dessa artiklar toocompare priser: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> och <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* Du kan bara använda SQL Server-versioner som stöds av SAP, t.ex. SQL Server 2012.
* hello sorteringen av hello SQL Server-instansen, som installeras i hello virtuella datorer som erbjuds i hello Azure Marketplace är hello sorteringen SAP NetWeaver kräver hello SQL Server-instansen toorun. Du kan ändra hello sorteringen men med hello anvisningarna i följande avsnitt hello.

#### <a name="changing-hello-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Ändra hello SQL Server-sorteringen för en Microsoft Windows/SQL Server-VM
Eftersom hello SQL Server-avbildningar i hello Azure Marketplace inte är inställda toouse hello sortering, vilket krävs av SAP NetWeaver program måste toobe ändras direkt efter hello distributionen. För SQL Server 2012, detta kan göras med hello följande steg så snart hello VM har distribuerats och en administratör kan toolog till hello distribueras VM:

* Öppna Kommandotolken Windows 'som administratör ”.
* Ändra hello directory tooC:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Köra hello-kommandot: Setup.exe /QUIET/Action = REBUILDDATABASE /INSTANCENAME = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
  * `<local_admin_account_name`> är hello konto, som har definierats som hello administratörskonto när du distribuerar hello VM för hello första gången via hello-galleriet.

hello processen tar bara några minuter. Utför hello följa stegen i ordning toomake att om hello steg avslutas med hello korrekta resultat:

* Öppna SQL Server Management Studio.
* Öppna ett frågefönster.
* Köra hello kommandot sp_helpsort i hello SQL Server-huvuddatabasen.

hello önskade resultatet bör se ut som:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Om detta inte är hello resultatet stoppa distribuera SAP och undersöka varför hello installationskommandot inte fungerar som förväntat. Distribution av SAP NetWeaver program på SQL Server-instansen med olika SQL Server-kodsidor än hello som nämns ovan är **inte** stöds.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server hög tillgänglighet för SAP i Azure
Som nämnts tidigare i det här dokumentet, finns det ingen möjlighet toocreate delad lagring som är nödvändig för hello användning av hello äldsta SQL Server med hög tillgänglighet-funktioner. Den här funktionen installerar två eller flera SQL Server-instanser i ett Failover Cluster WSFC (Windows Server) med hjälp av en delad disk för hello användardatabaser (och slutligen tempdb). Detta är hello länge standard hög tillgänglighet, som även stöds av SAP. Eftersom Azure inte stöder delad lagring, kan SQL Server med hög tillgänglighet konfigurationer med en delad disk klusterkonfiguration realiseras. Många andra metoder för hög tillgänglighet är fortfarande möjligt och beskrivs i följande avsnitt hello.

#### <a name="sql-server-log-shipping"></a>SQLServer-Loggöverföring
En av hello hög tillgänglighet (HA) är SQL Server-Loggöverföring. Om hello virtuella datorer som deltar i hello konfiguration för hög tillgänglighet har fungerar namnmatchning, utan problem och hello installationen i Azure skiljer sig inte från några inställningar som görs lokalt. Det rekommenderas inte toorely på endast IP-lösning. Med angående toosetting Loggöverföring och hello principer runt Loggöverföring, hittar du den här dokumentationen:

<https://docs.microsoft.com/SQL/Database-Engine/log-Shipping/About-log-Shipping-SQL-Server>

I ordning tooreally uppnå inställningarna för hög tillgänglighet, måste toodeploy hello virtuella datorer som är inom sådana en Loggöverföring configuration toobe inom hello samma Tillgänglighetsuppsättning av typen Azure.

#### <a name="database-mirroring"></a>Databasspegling
Databasen spegling som stöds av SAP (Se SAP-kommentar [965908]) förlitar sig på definierar redundanspartner i hello SAP anslutningssträngen. Hello mellan lokala fall antar vi att hello två virtuella datorerna är i hello samma domän och att hello användarkontext hello två SQL Server-instanser körs under en domänanvändare och har behörighet i hello två SQL Server-instanser ingår. Därför hello installationen av databasspegling i Azure skiljer sig inte mellan en typisk lokalt/installationsprogrammet.

Som molnbaserad distributioner hello enklast toohave en annan domän konfigurera i Azure toohave dessa DBMS virtuella datorer (och helst dedikerade SAP virtuella datorer) inom en domän.

Om en domän inte är möjligt kan också använda certifikat för hello databasspegling slutpunkter som beskrivs här: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

En självstudiekurs tooset in databasspegling i Azure hittar du här: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>SQLServer Always On
Som alltid på stöds för SAP lokal (Se SAP-kommentar [1772688]), är det stöds toobe används tillsammans med SAP i Azure. hello faktum att du inte kan toocreate delade diskar i Azure innebär inte att en går inte att skapa en alltid på Failover Cluster WSFC (Windows Server) konfiguration mellan olika virtuella datorer. Det innebär bara att du inte har hello möjligheten toouse en delad disk som ett kvorum i hello klusterkonfiguration. Därför kan du skapa en alltid på WSFC-konfiguration i Azure och inte bara välja hello kvorum som använder delad disk. hello Azure-miljön dessa virtuella datorer distribueras i ska matcha hello virtuella datorer efter namn och hello virtuella datorer måste vara i hello samma domän. Detta gäller endast Azure och mellan lokala distributioner. Det finns vissa saker runt distribuera hello SQL Server tillgänglighetsgruppens lyssnare (inte toobe förväxlas med hello Azure Tillgänglighetsuppsättning) eftersom Azure vid denna tidpunkt inte tillåter toosimply skapa AD/DNS-objekt som det är möjligt lokalt. Vissa olika installationssteg är därför nödvändiga tooovercome hello specifikt beteende i Azure.

Vissa aspekter med hjälp av en Tillgänglighetsgruppslyssnare är:

* Med hjälp av en Tillgänglighetsgruppslyssnare är bara möjligt med Windows Server 2012 eller högre som gäst OS av hello VM. Du måste toomake till att korrigeringsfilen används för Windows Server 2012: <https://support.microsoft.com/kb/2854082> 
* För Windows Server 2008 R2 korrigeringsfilen finns inte och alltid på behöver toobe används i hello samma sätt som databasspegling genom att ange redundanspartner i hello anslutningar sträng (via hello SAP default.pfl parametern dbs/mss/server – Se SAP-kommentar [965908]).
* När med hjälp av en Tillgänglighetsgruppslyssnare, hello databasen virtuella datorer behöver toobe anslutna dedikerade tooa belastningsutjämnaren. Namnmatchning i distributioner för endast molnbaserad antingen kräver alla virtuella datorer i en SAP system (programservrar, DBMS-servern och server (A) SCS) är i hello samma virtuella nätverk eller kräver från en SAP layer hello programunderhåll hello etc\host filen i ordning tooget hello VM namnen på hello SQL Server-datorer som löst. I ordning tooavoid Azure tilldelar nya IP-adresser i de fall där båda VM: ar tillfälligtvis avstängning, ska en tilldela statiska IP-adresser toohello nätverksgränssnitt för dessa virtuella datorer i hello Always On-konfiguration (definierar en statisk IP-adress som anges i [detta] [ virtual-networks-reserved-private-ip] artikel)

[comment]: <> (Gamla bloggar)
[comment]: <> (< https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, < https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Det finns särskilda steg krävs när du skapar hello WSFC klusterkonfigurationen där hello klustret måste en särskild IP-adress tilldelas eftersom Azure med dess aktuella funktioner ska tilldela hello klusternamnet hello samma IP-adress som hello nod hello kluster skapas på. Det innebär att ett manuellt steg måste vara utförs tooassign ett annat toohello kluster för IP-adress.
* hello tillgänglighetsgruppens lyssnare kommer toobe som skapats i Azure med TCP/IP-slutpunkter som är tilldelade toohello virtuella datorer som kör hello primära och sekundära repliker av hello Availability group.
* Det kan finnas en toosecure behöver dessa slutpunkter med åtkomstkontrollistor.

[comment]: <> (Gamla TODO-blogg)
[comment]: <> (Hej detaljerade anvisningar och är nödvändigt att installera en AlwaysOn-konfigurationen på Azure är enklast att när igenom hello självstudiekursen tillgängliga [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[comment]: <> (Förkonfigurerade AlwaysOn-installationen via hello Azure-galleriet < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (Skapa en Tillgänglighetsgruppslyssnare är bäst beskrivs i kursen [this][virtual-machines-windows-classic-ps-sql-int-listener])
[comment]: <> (Att säkra nätverksslutpunkter med åtkomstkontrollistor beskrivs bästa här:)
[comment]: <> (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Det är möjligt toodeploy en SQL Server alltid på tillgänglighetsgrupp över samt olika Azure-regioner. Den här funktionen utnyttjar hello Azure VNet-till-Vnet-anslutningen ([mer][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (Gamla TODO-blogg)
[comment]: <> (hello installationen av SQL Server AlwaysOn-Tillgänglighetsgrupper i ett sådant scenario som beskrivs här: < https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>En sammanfattning av SQL Server med hög tillgänglighet i Azure
Angivna hello faktum att hello innehåll skyddas av Azure Storage finns en mindre orsak tooinsist på en hot standby-avbildning. Det innebär att ditt scenario med hög tillgänglighet måste tooonly skydda mot hello följande fall:

* Otillgänglighet hello VM som helhet förfallodatum toomaintenance hello server-kluster i Azure eller andra orsaker
* Problem med programvara i hello SQL Server-instansen
* Skydda från manuell fel där data tas bort och punkten i tidsåterställningen krävs

Titta på matchande tekniker en kan argumentera att hello första båda fallen kan omfattas av databasspegling eller alltid på hello tredje fallet endast kan omfattas av Loggöverföring.

Du behöver toobalance hello mer komplexa installationen av Always On jämfört med tooDatabase spegling med hello fördelarna med att alltid på. Dessa fördelar kan anges som:

* Läsbara sekundära repliker.
* Säkerhetskopiering från sekundära repliker.
* Bättre skalbarhet.
* Mer än en sekundära repliker.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Allmänna SQL Server för SAP på Azure sammanfattning
Det finns många rekommendationerna i den här guiden och vi rekommenderar att du läsa den mer än en gång innan du planerar distributionen av Azure. I allmänhet dock vara att toofollow hello översta tio allmänna DBMS på Azure specifika punkter:

[comment]: <> (2.3 högre genomströmning än vad? Än en VHD?)
1. Använd hello senaste DBMS-versionen, t.ex. SQL Server 2014 som har hello de flesta fördelar i Azure. För SQL Server är SQL Server 2012 SP1 CU4, vilket omfattar hello-funktionen för säkerhetskopiering visa Azure Storage. Dock tillsammans med SAP rekommenderar vi minst SQL Server 2014 SP1 CU1 eller SQL Server 2012 SP2 och hello senaste CU.
2. Planera noggrant liggande din SAP-system i Azure toobalance hello data filens layout och Azure begränsningar:
   * Inte har för många diskar, men har tillräckligt med tooensure som du kan nå dina krävs IOPS.
   * Kom ihåg IOPS är också begränsad per Azure Storage-konto och att Storage-konton är begränsade i varje Azure-prenumeration om du inte använder hanterade diskar ([mer][azure-subscription-service-limits]). 
   * Endast stripe över diskar om du behöver tooachieve en högre genomströmning.
3. Installera aldrig programvara eller placera alla filer som kräver beständiga på hello D:\ enhet som den är inte permanent och allt på den här enheten går förlorade vid en omstart av Windows.
4. Använd inte cachelagring på disk för Azure standardlagring.
5. Använd inte Azure georeplikerad Storage-konton.  Använd lokalt Redundant för DBMS-arbetsbelastningar.
6. Använd DBMS leverantörens hr/DR lösning tooreplicate databasdata.
7. Alltid använda namnmatchning, Använd inte IP-adresser.
8. Använd hello högsta databasen komprimering möjligt. Detta är sidan komprimering för SQL Server.
9. Var försiktig med hjälp av SQL Server-avbildningar från hello Azure Marketplace. Om du använder hello SQL Server en, måste du ändra sorteringen av hello-instansen innan du installerar alla SAP NetWeaver system på den.
10. Installera och konfigurera hello SAP värden för övervakning av Azure enligt beskrivningen i [Deployment Guide][deployment-guide].

## <a name="specifics-toosap-ase-on-windows"></a>Närmare information tooSAP ASE i Windows
Från och med Microsoft Azure kan migrera du enkelt dina befintliga SAP ASE program tooAzure virtuella datorer. SAP ASE i en virtuell dator kan du tooreduce hello totala ägandekostnaden för distribution, hantering och underhåll av breda företagsprogram genom att enkelt migrera dessa program tooMicrosoft Azure. Med SAP ASE i en virtuell dator i Azure, kan administratörer och utvecklare fortfarande använda hello samma utvecklings- och administrationskostnader verktyg som är tillgängliga på lokalt.

Det finns ett SLA för hello Azure virtuella datorer som finns här: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Vi är säker på att Microsoft Azure virtuella värddatorerna utför mycket bra i virtualiseringslösningar för jämförelse tooother offentliga moln, men enskilda resultaten kan variera. SAP storleksanpassa SAP antal hello olika SAP certifierade VM SKU: er finns i en separat SAP-kommentar [1928533].

Instruktioner och rekommendationerna i beaktande toohello användning av Azure Storage, distribution av SAP virtuella datorer eller SAP övervakning gäller toodeployments för SAP ASE tillsammans med SAP-program som anges i hela hello första fyra kapitlen i det här dokumentet.

### <a name="sap-ase-version-support"></a>Stöd för SAP-ASE Version
SAP för närvarande stöder SAP ASE version 16,0 för SAP Business Suite. Alla uppdateringar för SAP ASE server eller JDBC- och ODBC-drivrutiner toobe används med SAP Business Suite produkter sker enbart via hello SAP Service Marketplace på: <https://support.sap.com/swdc>.

För installationer lokalt, att inte hämta uppdateringar för hello SAP ASE servern eller för hello JDBC och ODBC-drivrutiner direkt från Sybase-webbplatser. Mer detaljerad information om korrigeringsprogram som stöds för användning med SAP Business Suite produkter lokalt och i Azure Virtual Machines finns hello följande SAP anteckningar:

* [1590719]
* [1973241]

Allmän information om hur du kör SAP Business Suite på SAP ASE kan hittas i hello [Tillståndsändringsavisering](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Riktlinjer för SAP ASE konfiguration för SAP-relaterade SAP ASE installationer i virtuella Azure-datorer
#### <a name="structure-of-hello-sap-ase-deployment"></a>Struktur för hello ASE SAP-distribution
I enlighet med hello allmän beskrivning och SAP ASE körbara filer finns eller installerats i hello systemenhet hello VM OS-disken (enhet c:\). Vanligtvis utnyttjas de flesta av hello SAP ASE system och verktyg databaser inte verkligen hårddisk av SAP NetWeaver arbetsbelastning. Därför hello system och verktyg databaser (master, model, saptools, sybmgmtdb, sybsystemdb) kan finnas kvar på hello C:\ samt enhet. 

Ett undantag kunde hello temporära databasen som innehåller alla arbetsobjekt tabeller och temporära tabeller som skapats av SAP ASE som vid vissa SAP ERP och alla BW arbetsbelastningar kan kräva högre datavolym eller i/o operations volymen, som inte får plats i hello ursprungliga Virtuella datorer med OS-disken (enhet c:\).

Beroende på hello version av SAPInst/SWPM används tooinstall hello system, hello databasen kan innehålla:

* En enda SAP ASE tempdb som skapas när du installerar SAP ASE
* En SAP ASE tempdb som skapats genom att installera SAP ASE och en ytterligare saptempdb som skapats av hello rutin för SAP-installation
* En SAP ASE tempdb som skapats genom att installera SAP ASE och en ytterligare tempdb som har skapats manuellt (till exempel följande SAP-kommentar [1752266]) toomeet ERP/BW specifika tempdb-krav

Vid specifika ERP eller alla BW arbetsbelastningar klokt det, i beaktande tooperformance tookeep hello tempdb enheter hello dessutom skapa tempdb (via SWPM eller manuellt) på en annan enhet än C:\. Om det inte finns några ytterligare tempdb rekommenderas toocreate en (SAP-kommentar [1752266]).

Följande steg ska utföras för hello dessutom skapa tempdb för sådana system hello:

* Flytta hello första tempdb enheten toohello första enhet hello SAP-databasen
* Lägg till tempdb enheter tooeach av hello virtuella hårddiskar som innehåller en enhet för hello SAP-databasen

Den här konfigurationen möjliggör tempdb tooeither förbruka mer utrymme än hello systemenhet är kan tooprovide. Som referens Kontrollera en hello tempdb enheten storlekar på befintliga system som kör lokalt. Eller en sådan konfiguration skulle aktivera IOPS siffror mot tempdb som kan tillhandahållas med hello systemenheten. System som körs lokalt kan igen att använda toomonitor i/o-arbetsbelastning mot tempdb.

Placera inte alla enheter som SAP ASE till hello D:\ enhet hello VM. Detta gäller även toohello tempdb, även om hello objekt sparas i hello tempdb är temporära.

#### <a name="impact-of-database-compression"></a>Effekten av databasen komprimering
Alla åtgärder, vilket minskar IOPS kan hjälpa toostretch hello arbetsbelastningen kan köra i ett IaaS-scenario som Azure i konfigurationer där i/o-bandbredd kan bli en begränsande faktor. Därför rekommenderas toomake till att SAP ASE komprimering används innan du laddar upp en befintlig tooAzure för SAP-databasen.

hello rekommendation tooperform komprimering innan du laddar upp tooAzure om den inte redan har implementerats får slut på flera orsaker:

* hello mängden data överförs toobe tooAzure är lägre
* hello varaktighet hello komprimering körningen är kortare förutsatt att något kan använda starkare maskinvara med flera processorer eller högre i/o-bandbredd eller mindre i/o-svarstid lokalt
* Databasen är mindre leda tooless kostnader för diskallokering

Data och LOB komprimering fungerar på en virtuell dator som finns i Azure Virtual Machines som sker lokalt. Mer information om hur toocheck om komprimering används redan i en befintlig databas i SAP ASE kontrollera SAP-kommentar [1750510].

#### <a name="using-dbacockpit-toomonitor-database-instances"></a>Med hjälp av DBACockpit toomonitor Databasinstanser
För SAP-system som använder SAP ASE som databasplattform, är Hej DBACockpit tillgänglig som inbäddade webbläsarfönster i transaktion DBACockpit eller Webdynpro. Men hello full funktionalitet för övervakning och administration hello-databasen är tillgänglig i hello Webdynpro implementeringen av hello DBACockpit.

Som kräver lokalt system är flera steg tooenable alla SAP NetWeaver-funktioner som används av hello Webdynpro implementering av hello DBACockpit. Följ SAP-kommentar [1245200] tooenable hello användning av webdynpros och generera de som krävs för hello. När följande hello instruktionerna i hello ovan anteckningar konfigurera du också hello hanteraren för Internet-kommunikation (icm) tillsammans med hello portar toobe används för http och https-anslutningar. hello standardinställningen för HTTP-ser ut så här:

> ICM/server_port_0 = skydd = HTTP, PORT = 8000 PROCTIMEOUT = 600, TIMEOUT = 600
> 
> ICM/server_port_1 = skydd = HTTPS, PORT = 443$ $PROCTIMEOUT = 600, TIMEOUT = 600
> 
> 

och hello länkar genereras i transaktion DBACockpit ser liknande toothis:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Beroende på om och hur hello Azure-dator värd hello SAP-systemet är ansluten via plats-till-plats, flera platser eller ExpressRoute (mellan lokala distribution) behöver du toomake till den ICM använder ett fullständigt kvalificerat värdnamn som kan lösas på hello datorn där du försöker tooopen hello DBACockpit från. Se SAP-kommentar [773830] toounderstand hur ICM anger hello fullt kvalificerat värdnamn beroende på profilen parametrar och ange parametern icm/host_name_full uttryckligen om det behövs.

Om du har distribuerat hello VM i ett scenario med endast molnbaserad utan anslutning mellan lokala och Azure måste toodefine en offentlig IP-adress och en domainlabel. hello format för hello offentliga DNS-namnet på hello VM ser ut så här:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

Mer information om relaterade toohello DNS-namn kan hittas [här][virtual-machines-azurerm-versus-azuresm].

Ange hello SAP profil parametern icm/host_name_full toohello DNS-namnet på hello Azure VM hello länken kan se ut:

> sap/https://mydomainlabel.westeurope.cloudapp.NET:44300/bc/sap/webdynpro/dba_cockpit
> 
> sap/http://mydomainlabel.westeurope.cloudapp.NET:8000/bc/sap/webdynpro/dba_cockpit
> 
> 

I det här fallet behöver du toomake se till att:

* Lägg till regler för inkommande trafik toohello Nätverkssäkerhetsgruppen i hello Azure portal för toocommunicate för hello TCP/IP-portar som används med ICM
* Lägg till regler för inkommande trafik toohello Windows-brandväggen för hello TCP/IP-portarna som används toocommunicate med hello ICM

För en automatiserad importeras av alla ändringar som är tillgänglig, rekommenderas tooperiodically gäller hello korrigering samling SAP-kommentar tillämpliga tooyour SAP version:

* [1558958]
* [1619967]
* [1882376]

Mer information om DBA Cockpit för SAP ASE finns i följande SAP anteckningar hello:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Säkerhetskopiering/återställning överväganden för SAP ASE
När du distribuerar SAP ASE till Azure måste din backup metod granskas. Även om hello system inte är en produktiv system måste hos SAP ASE hello SAP-databasen säkerhetskopieras regelbundet. Eftersom Azure Storage håller tre bilder, är nu en säkerhetskopia mindre viktiga avseende toocompensating en krasch lagring. hello främsta skälet för att bibehålla en korrekt plan för säkerhetskopiering och återställning är mer som du kan kompensera för logiska/manuell fel genom att tillhandahålla punkt i tiden återställningsfunktioner. Så hello målet är tooeither använda säkerhetskopior toorestore hello databas tillbaka tooa vissa peka i tid eller toouse hello säkerhetskopieringar i Azure tooseed ett annat system genom att kopiera hello befintlig databas. T.ex, du kan överföra från en 2-skikts SAP tooa 3-skikts system konfigurationsinställningar för hello samma system genom att återställa en säkerhetskopia.

Säkerhetskopiera och återställa en databas i Azure fungerar hello samma sätt som sker lokalt. Se SAP-information:

* [1588316]
* [1585981]

Mer information om att skapa dumpen konfigurationer och schemalägga säkerhetskopieringar. Beroende på din strategi och dina behov kan du konfigurera databasen och loggfilerna Dumpar toodisk till en befintlig hello-diskar eller Lägg till ytterligare en disk för hello säkerhetskopiering. tooreduce hello risk för dataförlust om ett fel, är det rekommenderade toouse en disk där det finns ingen databasenhet.

Förutom data och LOB erbjuder också komprimering SAP ASE säkerhetskopieringskomprimering. toooccupy mindre utrymme med hello-databasen och loggfilerna Dumpar det rekommenderas toouse säkerhetskopieringskomprimering. Mer information finns i SAP-kommentar [1588316]. Datumkomprimerar hello säkerhetskopiering är också viktigt tooreduce hello mängden data toobe överföras om du planerar toodownload säkerhetskopior eller virtuella hårddiskar som innehåller säkerhetskopiering Dumpar från hello Azure virtuella tooon lokala.

Använd inte enhet D:\ som mål för databas eller logg dumpen.

#### <a name="performance-considerations-for-backupsrestores"></a>Prestandaöverväganden för säkerhetskopiering/återställning
Säkerhetskopiering/återställning prestanda är beroende av hur många volymer kan läsas parallellt och vilka hello genomströmning av volymerna kanske som bare metal-distributioner. Hello processorförbrukningen som används av säkerhetskopieringskomprimering kan dessutom spela en viktig roll på virtuella datorer med bara in tooeight CPU-trådar. Därför kan anta en:

* hello färre hello antalet diskar som används toostore hello databasen enheter, hello mindre hello övergripande genomflöde i läsning
* Hej mindre hello antalet CPU-trådar i hello VM, hello allvarligare hello effekten av säkerhetskopieringskomprimering
* Hej färre mål (Stripe-kataloger, diskar) toowrite hello säkerhetskopiering till, hello mindre hello-dataflöde

tooincrease hello antalet mål toowrite toothere finns två alternativ som kan vara används kombinerad beroende på dina behov:

* Hello säkerhetskopiering målvolymen striping över flera monterade diskar i ordning tooimprove hello IOPS genomströmning på stripe volymen
* Skapa en dump konfiguration på SAP ASE nivå, som använder flera mål directory toowrite hello dump till

Striping av en volym över flera monterade diskar har diskuterats tidigare i den här guiden. Mer information om hur du använder flera kataloger i hello SAP ASE dump konfiguration finns i toohello dokumentation om lagrade proceduren sp_config_dump, vilket är används toocreate hello dump konfigurationen på hello [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Katastrofåterställning med virtuella Azure-datorer
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Datareplikering med SAP Sybase replikering av Server
Hello ger SAP Sybase replikering Server (SRS) SAP ASE en varmt vänteläge lösning tootransfer databasen transaktioner tooa fjärrplats asynkront. 

hello installationen och driften av SRS fungerar samt funktionellt på en virtuell dator som finns i Azure-tjänster för virtuella datorer som lokalt.

ASE HADR via SAP replikering Server är planerad med en framtida utgåva. Den testas med och publicerat för Microsoft Azure-plattformar så snart den är tillgänglig.

## <a name="specifics-toosap-ase-on-linux"></a>Närmare information tooSAP ASE på Linux
Från och med Microsoft Azure kan migrera du enkelt dina befintliga SAP ASE program tooAzure virtuella datorer. SAP ASE i en virtuell dator kan du tooreduce hello totala ägandekostnaden för distribution, hantering och underhåll av breda företagsprogram genom att enkelt migrera dessa program tooMicrosoft Azure. Med SAP ASE i en virtuell dator i Azure, kan administratörer och utvecklare fortfarande använda hello samma utvecklings- och administrationskostnader verktyg som är tillgängliga på lokalt.

För att distribuera virtuella datorer i Azure viktiga tooknow hello officiella SLA: er som finns här: <https://azure.microsoft.com/support/legal/sla>

SAP mätinformationen och en lista över SAP certifierade VM SKU: er finns i SAP-kommentar [1928533]. Ytterligare SAP storleksanpassa dokument för Azure virtuella datorer finns här <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> och här <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Instruktioner och rekommendationerna i beaktande toohello användning av Azure Storage, distribution av SAP virtuella datorer eller SAP övervakning gäller toodeployments för SAP ASE tillsammans med SAP-program som anges i hela hello första fyra kapitlen i det här dokumentet.

hello innehåller följande två SAP anteckningar allmän information om ASE på Linux- och ASE i hello molnet:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Stöd för SAP-ASE Version
SAP för närvarande stöder SAP ASE version 16,0 för SAP Business Suite. Alla uppdateringar för SAP ASE server eller JDBC- och ODBC-drivrutiner toobe används med SAP Business Suite produkter sker enbart via hello SAP Service Marketplace på: <https://support.sap.com/swdc>.

För installationer lokalt, att inte hämta uppdateringar för hello SAP ASE servern eller för hello JDBC och ODBC-drivrutiner direkt från Sybase-webbplatser. Mer detaljerad information om korrigeringsprogram som stöds för användning med SAP Business Suite produkter lokalt och i Azure Virtual Machines finns hello följande SAP anteckningar:

* [1590719]
* [1973241]

Allmän information om hur du kör SAP Business Suite på SAP ASE kan hittas i hello [Tillståndsändringsavisering](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Riktlinjer för SAP ASE konfiguration för SAP-relaterade SAP ASE installationer i virtuella Azure-datorer
#### <a name="structure-of-hello-sap-ase-deployment"></a>Struktur för hello ASE SAP-distribution
I enlighet med hello allmän beskrivning och SAP ASE körbara filer finns eller installerats i hello rot filsystem hello VM (/sybase). Vanligtvis utnyttjas de flesta av hello SAP ASE system och verktyg databaser inte verkligen hårddisk av SAP NetWeaver arbetsbelastning. Därför hello system och verktyg databaser (master, model, saptools, sybmgmtdb, sybsystemdb) kan finnas kvar på hello rot filsystemet samt. 

Ett undantag kunde hello temporära databasen som innehåller alla arbetsobjekt tabeller och temporära tabeller som skapats av SAP ASE som vid vissa SAP ERP och alla BW arbetsbelastningar kan kräva högre datavolym eller i/o-åtgärder, volym som inte får plats i hello ursprungliga Virtuella datorer med OS-disken.

Beroende på hello version av SAPInst/SWPM används tooinstall hello system, hello databasen kan innehålla:

* En enda SAP ASE tempdb som skapas när du installerar SAP ASE
* En SAP ASE tempdb som skapats genom att installera SAP ASE och en ytterligare saptempdb som skapats av hello rutin för SAP-installation
* En SAP ASE tempdb som skapats genom att installera SAP ASE och en ytterligare tempdb som har skapats manuellt (till exempel följande SAP-kommentar [1752266]) toomeet ERP/BW specifika tempdb-krav

Vid specifika ERP eller alla BW arbetsbelastningar, är det klokt, i beaktande tooperformance tookeep hello tempdb enheter hello dessutom skapa tempdb (via SWPM eller manuellt) på en separat filsystem, som representeras av en enda Azure datadisk eller en Linux-RAID sträcker sig över flera diskar i Azure data. Om det inte finns några ytterligare tempdb rekommenderas toocreate en (SAP-kommentar [1752266]).

Följande steg ska utföras för hello dessutom skapa tempdb för sådana system hello:

* Flytta hello första tempdb directory toohello första filsystem hello SAP-databasen
* Lägg till tempdb kataloger tooeach hello diskar som innehåller ett filsystem i hello SAP-databasen

Den här konfigurationen möjliggör tempdb tooeither förbruka mer utrymme än hello systemenhet är kan tooprovide. Som referens Kontrollera en hello tempdb directory storlekar på befintliga system som kör lokalt. Eller en sådan konfiguration skulle aktivera IOPS siffror mot tempdb som kan tillhandahållas med hello systemenheten. System som körs lokalt kan igen att använda toomonitor i/o-arbetsbelastning mot tempdb.

Placera inte några kataloger som SAP ASE till /mnt eller /mnt/resource av hello VM. Detta gäller även toohello tempdb, även om hello objekt sparas i hello tempdb är temporära eftersom /mnt eller /mnt/resource är en standard Azure VM temp utrymme, vilket inte är beständig. Mer information om hello Azure VM temp utrymme finns i [i den här artikeln][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Effekten av databasen komprimering
Alla åtgärder, vilket minskar IOPS kan hjälpa toostretch hello arbetsbelastningen kan köra i ett IaaS-scenario som Azure i konfigurationer där i/o-bandbredd kan bli en begränsande faktor. Därför rekommenderas toomake till att SAP ASE komprimering används innan du laddar upp en befintlig tooAzure för SAP-databasen.

hello rekommendation tooperform komprimering innan du laddar upp tooAzure om den inte redan har implementerats får slut på flera orsaker:

* hello mängden data överförs toobe tooAzure är lägre
* hello varaktighet hello komprimering körningen är kortare förutsatt att något kan använda starkare maskinvara med flera processorer eller högre i/o-bandbredd eller mindre i/o-svarstid lokalt
* Databasen är mindre leda tooless kostnader för diskallokering

Data och LOB komprimering fungerar på en virtuell dator som finns i Azure Virtual Machines som sker lokalt. Mer information om hur toocheck om komprimering används redan i en befintlig databas i SAP ASE kontrollera SAP-kommentar [1750510]. För ytterligare information om databasen komprimering Se SAP-kommentar [2121797].

#### <a name="using-dbacockpit-toomonitor-database-instances"></a>Med hjälp av DBACockpit toomonitor Databasinstanser
För SAP-system som använder SAP ASE som databasplattform, är Hej DBACockpit tillgänglig som inbäddade webbläsarfönster i transaktion DBACockpit eller Webdynpro. Men hello full funktionalitet för övervakning och administration hello-databasen är tillgänglig i hello Webdynpro implementeringen av hello DBACockpit.

Som kräver lokalt system är flera steg tooenable alla SAP NetWeaver-funktioner som används av hello Webdynpro implementering av hello DBACockpit. Följ SAP-kommentar [1245200] tooenable hello användning av webdynpros och generera de som krävs för hello. När följande hello instruktionerna i hello ovan anteckningar konfigurera du också hello hanteraren för Internet-kommunikation (icm) tillsammans med hello portar toobe används för http och https-anslutningar. hello standardinställningen för HTTP-ser ut så här:

> ICM/server_port_0 = skydd = HTTP, PORT = 8000 PROCTIMEOUT = 600, TIMEOUT = 600
> 
> ICM/server_port_1 = skydd = HTTPS, PORT = 443$ $PROCTIMEOUT = 600, TIMEOUT = 600
> 
> 

och hello länkar genereras i transaktion DBACockpit ser liknande toothis:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Beroende på om och hur hello Azure-dator värd hello SAP-systemet är ansluten via plats-till-plats, flera platser eller ExpressRoute (mellan lokala distribution) behöver du toomake till den ICM använder ett fullständigt kvalificerat värdnamn som kan lösas på hello datorn där du försöker tooopen hello DBACockpit från. Se SAP-kommentar [773830] toounderstand hur ICM anger hello fullt kvalificerat värdnamn beroende på profilen parametrar och ange parametern icm/host_name_full uttryckligen om det behövs.

Om du har distribuerat hello VM i ett scenario med endast molnbaserad utan anslutning mellan lokala och Azure måste toodefine en offentlig IP-adress och en domainlabel. hello format för hello offentliga DNS-namnet på hello VM ser ut så här:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

Mer information om relaterade toohello DNS-namn kan hittas [här][virtual-machines-azurerm-versus-azuresm].

Ange hello SAP profil parametern icm/host_name_full toohello DNS-namnet på hello Azure VM hello länken kan se ut:

> sap/https://mydomainlabel.westeurope.cloudapp.NET:44300/bc/sap/webdynpro/dba_cockpit
> 
> sap/http://mydomainlabel.westeurope.cloudapp.NET:8000/bc/sap/webdynpro/dba_cockpit
> 
> 

I det här fallet behöver du toomake se till att:

* Lägg till regler för inkommande trafik toohello Nätverkssäkerhetsgruppen i hello Azure portal för toocommunicate för hello TCP/IP-portar som används med ICM
* Lägg till regler för inkommande trafik toohello Windows-brandväggen för hello TCP/IP-portarna som används toocommunicate med hello ICM

För en automatiserad importeras av alla ändringar som är tillgänglig, rekommenderas tooperiodically gäller hello korrigering samling SAP-kommentar tillämpliga tooyour SAP version:

* [1558958]
* [1619967]
* [1882376]

Mer information om DBA Cockpit för SAP ASE finns i följande SAP anteckningar hello:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Säkerhetskopiering/återställning överväganden för SAP ASE
När du distribuerar SAP ASE till Azure måste din backup metod granskas. Även om hello system inte är en produktiv system måste hos SAP ASE hello SAP-databasen säkerhetskopieras regelbundet. Eftersom Azure Storage håller tre bilder, är nu en säkerhetskopia mindre viktiga avseende toocompensating en krasch lagring. hello främsta skälet för att bibehålla en korrekt plan för säkerhetskopiering och återställning är mer som du kan kompensera för logiska/manuell fel genom att tillhandahålla punkt i tiden återställningsfunktioner. Så hello målet är tooeither använda säkerhetskopior toorestore hello databas tillbaka tooa vissa peka i tid eller toouse hello säkerhetskopieringar i Azure tooseed ett annat system genom att kopiera hello befintlig databas. T.ex, du kan överföra från en 2-skikts SAP tooa 3-skikts system konfigurationsinställningar för hello samma system genom att återställa en säkerhetskopia.

Säkerhetskopiera och återställa en databas i Azure fungerar hello samma sätt som sker lokalt. Se SAP-information:

* [1588316]
* [1585981]

Mer information om att skapa dumpen konfigurationer och schemalägga säkerhetskopieringar. Beroende på din strategi och dina behov kan du konfigurera databasen och loggfilerna Dumpar toodisk till en befintlig hello-diskar eller Lägg till ytterligare en disk för hello säkerhetskopiering. tooreduce hello risk för dataförlust om ett fel är det rekommenderade toouse en disk där det finns ingen directory/databasfil.

Förutom data och LOB erbjuder också komprimering SAP ASE säkerhetskopieringskomprimering. toooccupy mindre utrymme med hello-databasen och loggfilerna Dumpar det rekommenderas toouse säkerhetskopieringskomprimering. Mer information finns i SAP-kommentar [1588316]. Datumkomprimerar hello säkerhetskopiering är också viktigt tooreduce hello mängden data toobe överföras om du planerar toodownload säkerhetskopior eller virtuella hårddiskar som innehåller säkerhetskopiering Dumpar från hello Azure virtuella tooon lokala.

Använd inte hello Azure VM temp utrymme /mnt eller /mnt/resource som mål för databas eller logg dumpen.

#### <a name="performance-considerations-for-backupsrestores"></a>Prestandaöverväganden för säkerhetskopiering/återställning
Säkerhetskopiering/återställning prestanda är beroende av hur många volymer kan läsas parallellt och vilka hello genomströmning av volymerna kanske som bare metal-distributioner. Hello processorförbrukningen som används av säkerhetskopieringskomprimering kan dessutom spela en viktig roll på virtuella datorer med bara in tooeight CPU-trådar. Därför kan anta en:

* hello färre hello antalet diskar som används toostore hello databasen enheter, hello mindre hello övergripande genomflöde i läsning
* Hej mindre hello antalet CPU-trådar i hello VM, hello allvarligare hello effekten av säkerhetskopieringskomprimering
* hello färre mål (Linux programvarubaserad RAID, diskar) toowrite Hej säkerhetskopiering till, hello mindre hello-dataflöde

tooincrease hello antalet mål toowrite toothere finns två alternativ som kan vara används kombinerad beroende på dina behov:

* Hello säkerhetskopiering målvolymen striping över flera monterade diskar i ordning tooimprove hello IOPS genomströmning på stripe volymen
* Skapa en dump konfiguration på SAP ASE nivå, som använder flera mål directory toowrite hello dump till

Striping av en volym över flera monterade diskar har diskuterats tidigare i den här guiden. Mer information om hur du använder flera kataloger i hello SAP ASE dump konfiguration finns i toohello dokumentation om lagrade proceduren sp_config_dump, vilket är används toocreate hello dump konfigurationen på hello [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Katastrofåterställning med virtuella Azure-datorer
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Datareplikering med SAP Sybase replikering av Server
Hello ger SAP Sybase replikering Server (SRS) SAP ASE en varmt vänteläge lösning tootransfer databasen transaktioner tooa fjärrplats asynkront. 

hello installationen och driften av SRS fungerar samt funktionellt på en virtuell dator som finns i Azure-tjänster för virtuella datorer som lokalt.

ASE HADR via SAP replikering Server stöds inte vid denna tidpunkt. Den kan testas med och publicerat för Microsoft Azure-plattformar i hello framtida.

## <a name="specifics-toooracle-database-on-windows"></a>Närmare information tooOracle databas i Windows
Oracle programvaran stöds av Oracle toorun på Microsoft Windows Hyper-V och Azure. Information om hello allmänt stöd för Windows Hyper-V och Azure, kontrollera: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Följande allmän hello-support hello situation för SAP-program som utnyttjar Oracle-databaser stöds också. Information om namnet i den här delen av hello dokument.

### <a name="oracle-version-support"></a>Stöd för Oracle-Version
Oracle och motsvarande OS-versioner som stöds för körs SAP på Oracle på Azure Virtual Machines kan hittas i SAP-kommentar [2039619].

Allmän information om hur du kör SAP Business Suite på Oracle kan hittas i 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Riktlinjer för Oracle-konfiguration för SAP installationer i virtuella Azure-datorer
#### <a name="storage-configuration"></a>Storage-konfiguration
Bara instans Oracle med NTFS-formaterade diskar stöds. Alla databasfiler måste lagras i hello NTFS-filsystemet baserat på virtuella hårddiskar eller hanterade diskar. Diskarna är monterade toohello virtuella Azure-datorn och baseras på Azure sidan BLOB Storage (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) eller hanterade diskar (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alla typer av nätverksenheter eller fjärresurser som Azure Filtjänster:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/Persisting-Connections-to-Microsoft-Azure-Files.aspx>

är **inte** stöd för Oracle-databasfiler!

Använda diskar baserat på Azure sidan BLOB Storage eller hanterade diskar, hello uppgifterna i det här dokumentet i kapitlet [cachelagring för virtuella datorer och datadiskar] [ dbms-guide-2.1] och [Microsoft Azure Storage] [ dbms-guide-2.3] gäller toodeployments med samt hello Oracle-databas.

Enligt beskrivningen tidigare i hello allmänna del hello dokumentet finns kvoter på IOPS genomströmning för Azure-diskarna. hello exakt kvoter används beroende på typ av hello VM. En lista över Virtuella typer med sina kvoter hittar [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows].

tooidentify hello Azure VM-typer som stöds finns i Obs tooSAP [1928533].

Så länge hello kvot på aktuella IOPS per disk uppfyller hello krav, det är möjligt toostore alla hello DB-filer på en enda monterade disken. 

Om fler IOPS krävs, rekommenderas starkt toouse fönstret lagringspooler (endast tillgängligt i Windows Server 2012 och senare) eller Windows striping av en stor logisk enhet över flera monterade diskar för Windows 2008 R2 toocreate (Se även kapitel [Programvara RAID] [ dbms-guide-2.2] i det här dokumentet). Den här metoden förenklar hello administration övergripande toomanage hello diskutrymme och undviker hello arbete toomanually distribuerar filer över flera monterade diskar.

#### <a name="backup--restore"></a>Säkerhetskopiering/återställning
För säkerhetskopiering / återställning av funktioner, hello SAP BR * verktyg för Oracle stöds i hello samma sätt som på standard operativsystemen Windows Server och Hyper-V. Oracle Recovery Manager (RMAN) har också stöd för toodisk för säkerhetskopiering och återställning från disk.

#### <a name="high-availability"></a>Hög tillgänglighet
Oracle Data Guard stöds för hög tillgänglighet och katastrofåterställning återställning. Information finns i [detta] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentation.

#### <a name="other"></a>Annat
Andra allmänna avsnitt som Azure-Tillgänglighetsuppsättningar eller SAP övervakning tillämpas enligt beskrivningen i hello tre första kapitlen i det här dokumentet för distributioner av virtuella datorer med samt hello Oracle-databas.

## <a name="specifics-toooracle-database-on-oracle-linux"></a>Närmare information tooOracle databasen på Oracle Linux
Oracle programvaran stöds av Oracle toorun på Microsoft Windows Hyper-V och Azure. Information om hello allmänt stöd för Windows Hyper-V och Azure, kontrollera: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Följande allmän hello-support hello situation för SAP-program som utnyttjar Oracle-databaser stöds också. Information om namnet i den här delen av hello dokument.

### <a name="oracle-version-support"></a>Stöd för Oracle-Version
Oracle och motsvarande OS-versioner som stöds för körs SAP på Oracle på Azure Virtual Machines kan hittas i SAP-kommentar [2039619].

Allmän information om hur du kör SAP Business Suite på Oracle kan hittas i 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Riktlinjer för Oracle-konfiguration för SAP installationer i virtuella Azure-datorer
#### <a name="storage-configuration"></a>Storage-konfiguration
Endast stöds instans Oracle med ext3, ext4 och xfs formaterade diskar. Alla databasfiler måste lagras i dessa filsystem som baseras på virtuella hårddiskar eller hanterade diskar. Diskarna är monterade toohello virtuella Azure-datorn och baseras på Azure sidan BLOB Storage (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) eller hanterade diskar (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alla typer av nätverksenheter eller fjärresurser som Azure Filtjänster:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/Persisting-Connections-to-Microsoft-Azure-Files.aspx>

är **inte** stöd för Oracle-databasfiler!

Använda diskar baserat på Azure sidan BLOB Storage eller hanterade diskar, hello uppgifterna i det här dokumentet i kapitlet [cachelagring för virtuella datorer och datadiskar] [ dbms-guide-2.1] och [Microsoft Azure Storage] [ dbms-guide-2.3] gäller toodeployments med samt hello Oracle-databas.

Enligt beskrivningen tidigare i hello allmänna del hello dokumentet finns kvoter på IOPS genomströmning för Azure-diskarna. hello exakt kvoter används beroende på typ av hello VM. En lista över Virtuella typer med sina kvoter hittar [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows].

tooidentify hello Azure VM-typer som stöds finns i Obs tooSAP [1928533]

Så länge hello kvot på aktuella IOPS per disk uppfyller hello krav, det är möjligt toostore alla hello DB-filer på en enda monterade disken. 

Om fler IOPS krävs, rekommenderas toouse LVM (logisk volymhanterare) eller MDADM toocreate en stor logisk volym över flera monterade diskar. Se även kapitel [programvara RAID] [ dbms-guide-2.2] i det här dokumentet. Den här metoden förenklar hello administration övergripande toomanage hello diskutrymme och undviker hello arbete toomanually distribuerar filer över flera monterade diskar.

#### <a name="backup--restore"></a>Säkerhetskopiering/återställning
För säkerhetskopiering / återställning av funktioner, hello SAP BR * verktyg för Oracle stöds i hello samma sätt som på utan operativsystem och Hyper-V. Oracle Recovery Manager (RMAN) har också stöd för toodisk för säkerhetskopiering och återställning från disk.

#### <a name="high-availability"></a>Hög tillgänglighet
Oracle Data Guard stöds för hög tillgänglighet och katastrofåterställning återställning. Information finns i [detta] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentation.

#### <a name="other"></a>Annat
Andra allmänna avsnitt som Azure-Tillgänglighetsuppsättningar eller SAP övervakning tillämpas enligt beskrivningen i hello tre första kapitlen i det här dokumentet för distributioner av virtuella datorer med samt hello Oracle-databas.

## <a name="specifics-for-hello-sap-maxdb-database-on-windows"></a>Programvarukrav för hello SAP MaxDB databas i Windows
### <a name="sap-maxdb-version-support"></a>Stöd för SAP-MaxDB Version
SAP för närvarande stöder SAP MaxDB version 7,9 för användning med SAP NetWeaver-baserade produkter i Azure. Alla uppdateringar för SAP MaxDB server eller JDBC- och ODBC-drivrutiner toobe används med SAP NetWeaver-baserade produkter sker enbart via hello SAP Service Marketplace på <https://support.sap.com/swdc>.
Allmän information om hur du kör SAP NetWeaver på SAP MaxDB finns på <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Microsoft Windows-versioner och Virtuella Azure-typer som stöds för SAP MaxDB DBMS
toofind hello stöds Microsoft Windows-version för SAP MaxDB DBMS på Azure, se:

* [SAP produkten tillgänglighet matris (PAM)][sap-pam]
* SAP-kommentar [1928533]

Det rekommenderas starkt toouse hello senaste versionen av hello operativsystemet Microsoft Windows, vilket är Microsoft Windows 2012 R2.

### <a name="available-sap-maxdb-documentation"></a>Tillgängliga SAP MaxDB dokumentation
Du kan visa hello uppdateras SAP MaxDB dokumentationen i hello följande SAP-kommentar [767598 ]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Riktlinjer för SAP MaxDB konfiguration för SAP installationer i virtuella Azure-datorer
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Lagringskonfigurationen
Metodtips för Azure-lagring för SAP MaxDB följer hello allmänna rekommendationer som anges i kapitel [struktur för en distribution av RDBMS][dbms-guide-2].

> [!IMPORTANT]
> Precis som andra databaser har SAP MaxDB också data och loggfiler. SAP MaxDB terminologi är dock hello rätt termen ”volym” (inte ”fil”). Det finns till exempel SAP MaxDB datavolymer och loggvolymer. Blanda inte ihop dem med OS-volymer. 
> 
> 

Kort sagt behöver du:

* Om du använder Azure Storage-konton, ange hello Azure storage-konto som innehåller hello SAP MaxDB data och loggfilen volymer (d.v.s. filer) för**lokalt Redundant lagring (LRS)** som anges i kapitel [Microsoft Azure Storage] [dbms-guide-2.3].
* Separata hello-i/o-sökväg för SAP MaxDB datavolymer (d.v.s. filer) från hello-i/o-sökväg för loggvolymer (d.v.s. filer). Det innebär att SAP MaxDB datavolymer (d.v.s. filer) har toobe installerad på en logisk enhet och SAP MaxDB loggvolymer (d.v.s. filer) har toobe installerad på en annan logisk enhet.
* Ange hello rätt cachelagring typ för varje disk, beroende på om du använder för SAP MaxDB data- eller loggfilen volymer (d.v.s. filer) och om du använder Azure-Standard eller Azure Premium-lagring, som beskrivs i kapitlet [cachelagring för virtuella datorer och datadiskar][dbms-guide-2.1].
* Så länge hello kvot på aktuella IOPS per disk uppfyller hello krav, är möjliga toostore alla hello datavolymer på en monterad disk och även lagra alla loggvolymer för databasen på en annan enkel monterade disken.
* Om det krävs mer IOPS och/eller blanksteg, rekommenderas starkt toouse Microsoft fönstret lagringspooler (endast tillgängligt i Microsoft Windows Server 2012 och senare) eller Microsoft Windows striping för Microsoft Windows 2008 R2 toocreate en stor logisk enhet över flera monterade diskar. Se även kapitel [programvara RAID] [ dbms-guide-2.2] i det här dokumentet. Den här metoden förenklar hello administration övergripande toomanage hello diskutrymme och undviker hello merarbete som manuellt distribuera filer över flera monterade diskar.
* Hello högsta IOPS-krav, kan du använda Azure Premium-lagring som är tillgänglig på DS-serien och GS-serien virtuella datorer.

![För referenskonfiguration av Azure IaaS-VM för SAP MaxDB DBMS][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Säkerhetskopiering och återställning
När du distribuerar SAP MaxDB till Azure måste du granska din backup metoder. Även om hello system inte är en produktiv system måste hos SAP MaxDB hello SAP-databasen säkerhetskopieras regelbundet. Eftersom Azure Storage håller tre bilder, är nu en säkerhetskopia mindre viktiga för att skydda datorn mot lagring problem och viktigare operativa och administrativa fel. hello främsta skälet för att bibehålla en korrekt säkerhetskopiering och återställning plan är så att du kan kompensera för logiska eller manuell fel genom att tillhandahålla tidpunkt i återställningsfunktioner. Så hello målet är tooeither Använd säkerhetskopior toorestore hello databasen tooa vissa peka i tid eller toouse hello säkerhetskopieringar i Azure tooseed ett annat system genom att kopiera hello befintlig databas. T.ex, du kan överföra från en 2-skikts SAP tooa 3-skikts system konfigurationsinställningar för hello samma system genom att återställa en säkerhetskopia.

Säkerhetskopiera och återställa en databas i Azure fungerar hello samma sätt som för lokalt system, så du kan använda standard SAP MaxDB säkerhetskopiering/återställning verktygen som beskrivs i ett av hello SAP MaxDB dokumentationen dokument som anges i SAP-kommentar [767598 ]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Prestandaöverväganden för säkerhetskopiering och återställning
Prestanda för säkerhetskopiering och återställning är beroende av hur många volymer kan läsas parallellt och hello genomflödet av volymerna i som bare metal-distributioner. Hello processorförbrukningen som används av säkerhetskopieringskomprimering kan dessutom spela en viktig roll på virtuella datorer med upp tooeight CPU-trådar. Därför kan anta en:

* Hej färre hello antalet diskar som används toostore hello databasen enheter, hello lägre hello övergripande läsa genomflöde
* Hej mindre hello antalet CPU-trådar i hello VM, hello allvarligare hello effekten av säkerhetskopieringskomprimering
* Hej färre mål (Stripe-kataloger, diskar) toowrite hello säkerhetskopiering till hello lägre hello genomflöde

tooincrease hello antalet mål toowrite att finns det två alternativ som du kan använda, eventuellt i kombination, beroende på dina behov:

* Trafikklass separata volymer för säkerhetskopiering
* Hello säkerhetskopiering målvolymen striping över flera monterade diskar i ordning tooimprove hello IOPS genomströmning på volymen stripe-diskar
* Med separata dedikerade logiska diskenheter för:
  * SAP MaxDB Säkerhetskopiera volymer (d.v.s. filer)
  * SAP MaxDB datavolymer (d.v.s. filer)
  * SAP MaxDB loggvolymer (d.v.s. filer)

Striping av en volym över flera monterade diskar har diskuterats tidigare i kapitlet [programvara RAID] [ dbms-guide-2.2] i det här dokumentet. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Andra
Andra allmänna avsnitt som Azure-Tillgänglighetsuppsättningar eller SAP övervakning gäller även enligt beskrivningen i hello tre första kapitlen i det här dokumentet för distributioner av virtuella datorer med hello MaxDB SAP-databas.
Andra SAP MaxDB-specifika inställningar är transparent tooAzure virtuella datorer och beskrivs i olika dokument som anges i SAP-kommentar [767598 ] och i anteckningarna SAP:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Programvarukrav för SAP liveCache i Windows
### <a name="sap-livecache-version-support"></a>SAP liveCache stöd för Version
Minimal version av SAP liveCache stöds i Azure Virtual Machines är **SAP LC/LCAPPS 10.0 SP 25** inklusive **liveCache 7.9.08.31** och **LCA Build 25**, släppt för **EhP 2 för SAP SCM 7.0** och högre.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Microsoft Windows-versioner och Virtuella Azure-typer som stöds för SAP liveCache DBMS
toofind hello stöds Microsoft Windows-version för SAP liveCache på Azure, se:

* [SAP produkten tillgänglighet matris (PAM)][sap-pam]
* SAP-kommentar [1928533]

Det rekommenderas starkt toouse hello senaste versionen av hello operativsystemet Microsoft Windows Server. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache riktlinjer för konfiguration för SAP installationer i virtuella Azure-datorer
#### <a name="recommended-azure-vm-types"></a>Rekommenderade Azure VM-typer
Har en stor påverkan på prestanda för SAP liveCache som SAP liveCache är ett program som utför enorma beräkningar, hello mängd och hastighet för RAM-minne och CPU. 

För hello Azure VM typer som stöds av SAP (SAP-kommentar [1928533]), alla virtuella processorresurser allokerade toohello VM backas upp av dedikerade fysiska processorresurser av hello hypervisor. Ingen överetablering (och därför ingen konkurrens om processorresurser) sker.

På samma sätt för alla Virtuella Azure-instansen som stöds av SAP är hello datorminnet 100% mappas toohello fysiskt minne – överetablering (överdrivet åtagande), till exempel inte används.

Från det här perspektivet rekommenderas toouse hello ny D-serien eller DS-serien (i kombination med Azure Premium Storage) Azure VM typ, eftersom de har 60% snabbare processorer än hello A-serien. För hello högsta RAM-minne och CPU belastning, kan du använda G-serien och GS-serien (i kombination med Azure Premium Storage) virtuella datorer med hello senaste Intel Xeon® processorn E5 v3-familjen, som har två gånger hello minne och fyra gånger hello solid-state drive lagring (SSD) av hello D / DS-serien.

#### <a name="storage-configuration"></a>Lagringskonfigurationen
Eftersom SAP liveCache är baserad på SAP MaxDB teknik, alla hello Azure storage metoder som rekommenderas för SAP MaxDB enligt kapitel [lagringskonfiguration] [ dbms-guide-8.4.1] gäller också för SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Dedikerade virtuella Azure-datorn för liveCache
Eftersom SAP liveCache använder sig mycket dataresurser, för effektiv användning rekommenderas toodeploy på en dedikerad Azure-dator. 

![Dedikerade virtuella Azure-datorn liveCache för produktiva användningsfall][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Säkerhetskopiera och återställ
Säkerhetskopiering och återställning, inklusive prestandaöverväganden, som beskrivs i hello relevanta SAP MaxDB kapitlen [säkerhetskopierar och återställer] [ dbms-guide-8.4.2] och [prestandaöverväganden för säkerhetskopiering och återställa][dbms-guide-8.4.3]. 

#### <a name="other"></a>Annat
Andra allmänna avsnitt som beskrivs i hello relevanta SAP MaxDB [detta] [ dbms-guide-8.4.4] kapitel. 

## <a name="specifics-for-hello-sap-content-server-on-windows"></a>Programvarukrav för hello SAP Content Server i Windows
hello SAP Content Server är en separat, server-baserade komponenten toostore innehåll, till exempel elektroniska dokument i olika format. hello SAP innehållsserver tillhandahålls av utvecklingen av teknik och är toobe som används mellan program för SAP-program. Den installeras på ett separat system. Vanliga innehåll är utbildning material och dokumentation från Knowledge datalager eller tekniska ritningar från hello mySAP PLM dokumenthanteringssystem. 

### <a name="sap-content-server-version-support"></a>Stöd för SAP innehållsserver Version
SAP stöder för närvarande:

* **SAP innehållsserver** med version **6.50 (och senare)**
* **SAP MaxDB version 7,9**
* **Microsoft IIS (Internet Information Server) version 8.0 (och senare)**

Det rekommenderas starkt toouse hello senaste versionen av SAP innehållsserver som för närvarande hello för att skriva det här dokumentet är **6.50 SP4**, och hello senaste versionen av **Microsoft IIS 8.5**. 

Kontrollerar hello stöds senaste versioner av SAP innehållsservern och Microsoft IIS i hello [SAP produkten tillgänglighet matris PAM-][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Microsoft Windows och Virtuella Azure-typer som stöds för SAP innehållsserver
toofind i Windows-version som stöds för SAP innehållsservern på Azure, se:

* [SAP produkten tillgänglighet matris (PAM)][sap-pam]
* SAP-kommentar [1928533]

Det rekommenderas starkt toouse hello senaste versionen av Microsoft Windows Server.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Riktlinjer för SAP innehållsserver konfiguration för SAP installationer i virtuella Azure-datorer
#### <a name="storage-configuration"></a>Lagringskonfigurationen
Om du konfigurerar SAP innehållsserver toostore filer i hello SAP MaxDB databasen alla Azure-lagring best practices rekommendation för SAP MaxDB enligt kapitel [lagringskonfiguration] [ dbms-guide-8.4.1] är också giltigt hello SAP innehållsserver scenariot. 

Om du konfigurerar SAP innehållsserver toostore filer i filsystemet hello, är det rekommenderade toouse dedikerade logisk enhet. Med lagringsutrymmen för Windows kan du tooalso öka logisk diskstorlek och IOPS dataflöde, enligt beskrivningen i kapitel [programvara RAID][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>SAP innehållsserver plats
SAP innehållsserver har distribuerats i hello toobe samma Azure-region och Azure VNET där hello SAP-systemet har distribuerats. Du är ledigt toodecide om du vill toodeploy SAP innehållsserver komponenter på en dedikerad Azure VM eller på hello där hello SAP-systemet körs på samma virtuella dator. 

![Dedikerad Azure VM för SAP Content Server][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP cacheplats Server
hello SAP Cache-Server är en ytterligare server-baserad komponent tooprovide åtkomst too(cached) lokalt. hello SAP cacheserver cachelagrar hello dokument för en SAP-innehållsserver. Detta är toooptimize nätverkstrafik om dokument har toobe hämta mer än en gång från olika platser. hello regel är att hello SAP Cache-Server har toobe fysiskt Stäng toohello klient som ansluter till hello SAP Cache-Server. 

Här har du två alternativ:

1. **Klienten är ett system för backend-SAP** om en serverdel SAP konfigurerade tooaccess SAP innehållsserver kan det SAP-systemet är en klient. Eftersom både SAP-systemet och SAP innehållsserver distribueras i hello samma Azure-region – i hello samma Azure-datacenter – de är fysiskt Stäng tooeach andra. Det finns därför ingen måste toohave en dedikerad SAP Cache-Server. SAP UI-klienter (SAP GUI eller en webbläsare) åtkomst hello SAP-systemet direkt och hello SAP-hämtar dokument från hello SAP innehållsserver.
2. **Klienten är en lokal webbläsare** hello SAP innehållsserver kan vara konfigurerade toobe komma åt direkt hello webbläsare. I det här fallet är en webbläsare som körs på lokala en klient hello SAP innehållsserver. Lokala datacenter och Azure-datacenter placeras på olika fysiska platser (helst Stäng tooeach som är andra). Ditt lokala datacenter är anslutna tooAzure via Azure plats-till-plats-VPN eller ExpressRoute. Även om båda alternativen erbjuder säker anslutning tooAzure för VPN-nätverk, erbjuder inte nätverk plats-till-plats-anslutning ett nätverk bandbredd och fördröjning SERVICENIVÅAVTAL mellan hello lokala datacenter och hello Azure-datacenter. toospeed in åtkomst toodocuments du göra något av följande hello:
   1. Installera SAP Cache Server lokalt, Stäng webbläsaren för toohello lokalt (alternativet på [detta] [ dbms-guide-900-sap-cache-server-on-premises] bild)
   2. Konfigurera Azure ExpressRoute, vilket ger en snabb och låg latens dedikerad nätverksanslutning mellan lokala datacenter och Azure-datacenter.

![Alternativet tooinstall SAP Cache Server lokalt][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Säkerhetskopiering/återställning
Om du konfigurerar hello SAP innehållsserver toostore filer i hello SAP MaxDB databasen hello säkerhetskopiering/återställning proceduren- och prestandaöverväganden som beskrivs i SAP MaxDB kapitel [säkerhetskopierar och återställer] [ dbms-guide-8.4.2] och kapitel [prestandaöverväganden för säkerhetskopiering och återställning][dbms-guide-8.4.3]. 

Om du konfigurerar hello SAP innehållsserver toostore filer i filsystemet hello kan tooexecute manuell säkerhetskopiering/återställning av hello hela filstrukturen där hello dokument finns. Liknande tooSAP MaxDB säkerhetskopiering/återställning, är det rekommenderade toohave en dedikerad volym för säkerhetskopiering ändamål. 

#### <a name="other"></a>Annat
Andra specifika inställningar för SAP innehållsserver transparent tooAzure virtuella datorer och beskrivs i olika dokument och SAP anteckningar:

* <https://Service.SAP.com/contentserver> 
* SAP-kommentar [1619726]  

## <a name="specifics-tooibm-db2-for-luw-on-windows"></a>Närmare information tooIBM DB2 för LUW i Windows
Du kan enkelt migrera dina befintliga SAP-program som körs på IBM DB2 för Linux, UNIX- och Windows (LUW) tooAzure virtuella datorer med Microsoft Azure. Med SAP på IBM DB2 för LUW kan administratörer och utvecklare fortfarande använda hello samma utvecklings- och administrationskostnader verktyg som är tillgängliga på lokalt.
Allmän information om hur du kör SAP Business Suite på IBM DB2 för LUW kan hittas i hello SAP Community nätverket Lagringsnodernas på <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

För ytterligare information och uppdateringar om SAP på DB2 för LUW på Azure Se SAP-kommentar [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 för Linux och UNIX stöd för Windows-Version
SAP på IBM DB2 för LUW på virtuella Microsoft Azure-tjänster stöds från och med DB2 version 10.5.

Information om stöds SAP-produkter och Virtuella Azure-typer finns tooSAP Observera [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 för Linux, UNIX- och riktlinjer för Windows-konfiguration för SAP installationer i virtuella Azure-datorer
#### <a name="storage-configuration"></a>Lagringskonfigurationen
Alla databasfiler måste lagras i hello NTFS-filsystem baserat på direkt anslutna diskar. Diskarna är monterade toohello virtuella Azure-datorn och baseras i Azure sidan BLOB Storage (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) eller hanterade diskar (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alla typer av nätverksenheter eller fjärresurser som hello följande Azure Filtjänster är **inte** stöds för databasfilerna: 

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/Persisting-Connections-to-Microsoft-Azure-Files.aspx>

Om du använder diskar baserat på Azure sidan BLOB Storage eller hanterade diskar hello uppgifterna i det här dokumentet i kapitlet [struktur för en distribution av RDBMS] [ dbms-guide-2] gäller även toodeployments med hello IBM DB2 för LUW databasen. 

Enligt beskrivningen tidigare i hello allmänna del hello dokumentet finns kvoter på IOPS genomströmning för diskar. hello exakt kvoter beror på hello VM används. En lista över Virtuella typer med sina kvoter hittar [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows].

Så länge hello kvot på aktuella IOPS per disk är tillräckligt, det är möjligt toostore hello alla databasfiler på en enda monterade disken. 

Prestanda finns överväganden också toochapter ”säkerhet och prestanda överväganden för databasen datakataloger” i guider för SAP-installationen.

Alternativt kan du använda Windows lagringspooler (endast tillgängligt i Windows Server 2012 och senare) eller Windows striping för Windows 2008 R2 som beskrivs i kapitel [programvara RAID] [ dbms-guide-2.2] i det här dokumentet toocreate en stor logiska enhet över flera diskar.
Du måste ange en fysisk disk sektorstorlek på 512 KB för hello diskar som innehåller hello DB2 lagringssökvägar för dina sapdata och saptmp kataloger. När du använder lagringspooler för Windows, måste du skapa hello lagringspooler manuellt via kommandoradsgränssnittet hello parametern ”-LogicalSectorSizeDefault”. Mer information finns i <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Säkerhetskopiera/återställa
hello säkerhetskopiering/återställning funktioner för IBM DB2 LUW stöds i hello samma sätt som på standard operativsystemen Windows Server och Hyper-V.

Du måste se till att du har en giltig databas strategi för säkerhetskopiering på plats. 

Säkerhetskopiering/återställning prestanda beror på hur många volymer kan läsas parallellt och vilka hello genomströmning av volymerna kan vara som bare metal-distributioner. Hello processorförbrukningen som används av säkerhetskopieringskomprimering kan dessutom spela en viktig roll på virtuella datorer med bara in tooeight CPU-trådar. Därför kan anta en:

* hello färre hello antalet diskar som används toostore hello databasen enheter, hello mindre hello övergripande genomflöde i läsning
* Hej mindre hello antalet CPU-trådar i hello VM, hello allvarligare hello effekten av säkerhetskopieringskomprimering
* Hej färre mål (Stripe-kataloger, diskar) toowrite hello säkerhetskopiering till hello lägre hello genomflöde

tooincrease hello antalet mål toowrite till, två alternativ kan vara används kombinerad beroende på dina behov:

* Hello säkerhetskopiering målvolymen striping över flera diskar i ordning tooimprove hello IOPS genomströmning på stripe volymen
* Flera säkerhetskopiering mål directory toowrite hello till

#### <a name="high-availability-and-disaster-recovery"></a>Hög tillgänglighet och katastrofåterställning
Microsoft Cluster Server (MSCS) stöds inte.

DB2 katastrofåterställning för hög tillgänglighet (HADR) stöds. Om hello virtuella datorer av hello konfiguration för hög tillgänglighet har fungerar namnmatchning kan skilja inte hello installationen i Azure från alla inställningar som görs lokalt. Det rekommenderas inte toorely på endast IP-lösning.

Använd inte Geo-replikering för hello storage-konton som lagrar hello databasen diskar. Mer information finns i toochapter [Microsoft Azure Storage] [ dbms-guide-2.3] och kapitel [hög tillgänglighet och katastrofåterställning med virtuella Azure-datorer] [ dbms-guide-3].

#### <a name="other"></a>Annat
Andra allmänna avsnitt som Azure-Tillgänglighetsuppsättningar eller SAP övervakning tillämpas enligt beskrivningen i hello tre första kapitlen i det här dokumentet för distributioner av virtuella datorer med IBM DB2 för LUW samt. 

Se även toochapter [allmänna SQL Server för SAP på Azure sammanfattning][dbms-guide-5.8].

## <a name="specifics-tooibm-db2-for-luw-on-linux"></a>Närmare information tooIBM DB2 för LUW på Linux
Du kan enkelt migrera dina befintliga SAP-program som körs på IBM DB2 för Linux, UNIX- och Windows (LUW) tooAzure virtuella datorer med Microsoft Azure. Med SAP på IBM DB2 för LUW kan administratörer och utvecklare fortfarande använda hello samma utvecklings- och administrationskostnader verktyg som är tillgängliga på lokalt. Allmän information om hur du kör SAP Business Suite på IBM DB2 för LUW kan hittas i hello SAP Community nätverket Lagringsnodernas på <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

För ytterligare information och uppdateringar om SAP på DB2 för LUW på Azure Se SAP-kommentar [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 för Linux och UNIX stöd för Windows-Version
SAP på IBM DB2 för LUW på virtuella Microsoft Azure-tjänster stöds från och med DB2 version 10.5.

Information om stöds SAP-produkter och Virtuella Azure-typer finns tooSAP Observera [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 för Linux, UNIX- och riktlinjer för Windows-konfiguration för SAP installationer i virtuella Azure-datorer
#### <a name="storage-configuration"></a>Lagringskonfigurationen
Alla databasfiler måste vara lagrad i ett filsystem som baseras på direkt anslutna diskar. Diskarna är monterade toohello virtuella Azure-datorn och baseras i Azure sidan BLOB Storage (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) eller hanterade diskar (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alla typer av nätverksenheter eller fjärresurser som hello följande Azure Filtjänster är **inte** stöds för databasfilerna:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/Persisting-Connections-to-Microsoft-Azure-Files.aspx>

Om du använder diskar baserat på Azure sidan BLOB Storage hello uppgifterna i det här dokumentet i kapitlet [struktur för en distribution av RDBMS] [ dbms-guide-2] gäller även toodeployments med hello IBM DB2 för LUW databasen.

Enligt beskrivningen tidigare i hello allmänna del hello dokumentet finns kvoter på IOPS genomströmning för diskar. hello exakt kvoter beror på hello VM används. En lista över Virtuella typer med sina kvoter hittar [här (Linux)] [ virtual-machines-sizes-linux] och [här (Windows)][virtual-machines-sizes-windows].

Så länge hello kvot på aktuella IOPS per disk är tillräckligt, det är möjligt toostore hello alla databasfiler på en enda disk.

Prestanda finns överväganden också toochapter ”säkerhet och prestanda överväganden för databasen datakataloger” i guider för SAP-installationen.

Du kan också använda LVM (logisk volymhanterare) eller MDADM som beskrivs i kapitlet [programvara RAID] [ dbms-guide-2.2] för det här dokumentet toocreate en stor logisk enhet över flera diskar.
Du måste ange en fysisk disk sektorstorlek på 512 KB för hello diskar som innehåller hello DB2 lagringssökvägar för dina sapdata och saptmp kataloger.

#### <a name="backuprestore"></a>Säkerhetskopiera/återställa
hello säkerhetskopiering/återställning funktioner för IBM DB2 LUW stöds i hello samma sätt som på standard Linux installation lokalt.

Du måste se till att du har en giltig databas strategi för säkerhetskopiering på plats.

Säkerhetskopiering/återställning prestanda beror på hur många volymer kan läsas parallellt och vilka hello genomströmning av volymerna kan vara som bare metal-distributioner. Hello processorförbrukningen som används av säkerhetskopieringskomprimering kan dessutom spela en viktig roll på virtuella datorer med bara in tooeight CPU-trådar. Därför kan anta en:

* hello färre hello antalet diskar som används toostore hello databasen enheter, hello mindre hello övergripande genomflöde i läsning
* Hej mindre hello antalet CPU-trådar i hello VM, hello allvarligare hello effekten av säkerhetskopieringskomprimering
* Hej färre mål (Stripe-kataloger, diskar) toowrite hello säkerhetskopiering till hello lägre hello genomflöde

tooincrease hello antalet mål toowrite till, två alternativ kan vara används kombinerad beroende på dina behov:

* Hello säkerhetskopiering målvolymen striping över flera diskar i ordning tooimprove hello IOPS genomströmning på stripe volymen
* Flera säkerhetskopiering mål directory toowrite hello till

#### <a name="high-availability-and-disaster-recovery"></a>Hög tillgänglighet och katastrofåterställning
DB2 katastrofåterställning för hög tillgänglighet (HADR) stöds. Om hello virtuella datorer av hello konfiguration för hög tillgänglighet har fungerar namnmatchning kan skilja inte hello installationen i Azure från alla inställningar som görs lokalt. Det rekommenderas inte toorely på endast IP-lösning.

Använd inte Geo-replikering för hello storage-konton som lagrar hello databasen diskar. Mer information finns i toochapter [Microsoft Azure Storage] [ dbms-guide-2.3] och kapitel [hög tillgänglighet och katastrofåterställning med virtuella Azure-datorer] [ dbms-guide-3].

#### <a name="other"></a>Annat
Andra allmänna avsnitt som Azure-Tillgänglighetsuppsättningar eller SAP övervakning tillämpas enligt beskrivningen i hello tre första kapitlen i det här dokumentet för distributioner av virtuella datorer med IBM DB2 för LUW samt.

Se även toochapter [allmänna SQL Server för SAP på Azure sammanfattning][dbms-guide-5.8].

