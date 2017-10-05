---
title: "Azure virtuella datorer hög tillgänglighet för SAP NetWeaver | Microsoft Docs"
description: "Hög tillgänglighet guide för SAP NetWeaver på Azure Virtual Machines"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/07/2016
ms.author: goraco
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 65236f527b62b4990b062fb6a54ce13b3c182e93
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a><span data-ttu-id="96390-103">Hög tillgänglighet för SAP NetWeaver på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="96390-103">High availability for SAP NetWeaver on Azure VMs</span></span>

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

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:https://docs.microsoft.com/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md
[dbms-guide-2.1]:../../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:../../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:../../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:../../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:../../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:../../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:../../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:../../virtual-machines-windows-sap-deployment-guide.md
[deployment-guide-2.2]:../../virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:../../virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:../../virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:../../virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:../../virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:../../virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:../../virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:../../virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:../../virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:../../virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:../../virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:../../virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:../../virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:../../virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:../../virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:../../virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:../../virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:../../virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:../../virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:../../virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:../../virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:../../virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:../../virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:../../virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:../../virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:../../virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:../../virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:../../virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../../../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../../../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:../../virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:../../virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:../../virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:../../virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:high-availability-guide.md

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

[sap-ha-guide]:high-availability-guide.md
[sap-ha-guide-1]:high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c
[sap-ha-guide-2]:high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-3]:high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1
[sap-ha-guide-3.1]:high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686
[sap-ha-guide-3.2]:high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860
[sap-ha-guide-4]:high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-4.1]:high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2
[sap-ha-guide-5]:high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275
[sap-ha-guide-5.1]:high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592
[sap-ha-guide-5.2]:high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd
[sap-ha-guide-6]:high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a
[sap-ha-guide-6.1]:high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341
[sap-ha-guide-6.2]:high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb
[sap-ha-guide-7]:high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa
[sap-ha-guide-7.1]:high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e
[sap-ha-guide-7.2]:high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db
[sap-ha-guide-7.2.1]:high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51
[sap-ha-guide-7.3]:high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027
[sap-ha-guide-7.4]:high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816
[sap-ha-guide-8]:high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.2]:high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310
[sap-ha-guide-8.3]:high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a
[sap-ha-guide-8.4]:high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e
[sap-ha-guide-8.5]:high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f
[sap-ha-guide-8.6]:high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30
[sap-ha-guide-8.7]:high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e
[sap-ha-guide-8.8]:high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.10]:high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.2]:high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca
[sap-ha-guide-8.12.2.1]:high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855
[sap-ha-guide-8.12.2.2]:high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d
[sap-ha-guide-8.12.3]:high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-ha-guide-9.1.2]:high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0
[sap-ha-guide-9.1.3]:high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-ha-guide-9.1.4]:high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-ha-guide-9.1.5]:high-availability-guide.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-ha-guide-9.2]:high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7
[sap-ha-guide-9.3]:high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748
[sap-ha-guide-9.4]:high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-ha-guide-9.5]:high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5
[sap-ha-guide-9.6]:high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772
[sap-ha-guide-10]:high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9
[sap-ha-guide-10.1]:high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445
[sap-ha-guide-10.2]:high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916
[sap-ha-guide-10.3]:high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5

[sap-ha-multi-sid-guide]:high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


[sap-ha-guide-figure-1000]:media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
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
[virtual-machines-windows-attach-disk-portal]:../../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../azure-resource-manager/resource-group-overview.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../virtual-machines-windows-sizes.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md
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
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


<span data-ttu-id="96390-109">Azure Virtual Machines är lösningen för organisationer som behöver bearbetning, lagring och nätverksresurser, minimalt, och utan långa inköp cykler.</span><span class="sxs-lookup"><span data-stu-id="96390-109">Azure Virtual Machines is the solution for organizations that need compute, storage, and network resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="96390-110">Du kan använda Azure virtuella datorer för att distribuera klassiska program som SAP NetWeaver-baserade ABAP, Java och en ABAP + Java-stacken.</span><span class="sxs-lookup"><span data-stu-id="96390-110">You can use Azure Virtual Machines to deploy classic applications like SAP NetWeaver-based ABAP, Java, and an ABAP+Java stack.</span></span> <span data-ttu-id="96390-111">Utöka tillförlitlighet och tillgänglighet utan ytterligare lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="96390-111">Extend reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="96390-112">Virtuella Azure-datorer stöder korsanslutningar, så du kan integrera Azure virtuella datorer i din organisations lokala domäner, privata moln och SAP system liggande.</span><span class="sxs-lookup"><span data-stu-id="96390-112">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="96390-113">I den här artikeln beskriver vi de steg som du kan vidta för att distribuera hög tillgänglighet SAP-system i Azure med hjälp av Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="96390-113">In this article, we cover the steps that you can take to deploy high-availability SAP systems in Azure by using the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="96390-114">Vi går du igenom dessa viktiga uppgifter:</span><span class="sxs-lookup"><span data-stu-id="96390-114">We walk you through these major tasks:</span></span>

* <span data-ttu-id="96390-115">Hitta rätt SAP-information och riktlinjer för installation, som anges i den [resurser] [ sap-ha-guide-2] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="96390-115">Find the right SAP Notes and installation guides, listed in the [Resources][sap-ha-guide-2] section.</span></span> <span data-ttu-id="96390-116">Den här artikeln kompletterar SAP dokumentationen och SAP anteckningar, vilka är de primära resurser som hjälper dig att installera och distribuera program på vissa plattformar.</span><span class="sxs-lookup"><span data-stu-id="96390-116">This article complements SAP installation documentation and SAP Notes, which are the primary resources that can help you install and deploy SAP software on specific platforms.</span></span>
* <span data-ttu-id="96390-117">Lär dig skillnaderna mellan Azure Resource Manager-distributionsmodellen och Azure klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="96390-117">Learn the differences between the Azure Resource Manager deployment model and the Azure classic deployment model.</span></span>
* <span data-ttu-id="96390-118">Läs mer om Windows Server Failover Clustering kvorum läge, så du kan välja den modell som passar din Azure-distribution.</span><span class="sxs-lookup"><span data-stu-id="96390-118">Learn about Windows Server Failover Clustering quorum modes, so you can select the model that is right for your Azure deployment.</span></span>
* <span data-ttu-id="96390-119">Läs mer om Windows Server Failover Clustering delad lagring i Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="96390-119">Learn about Windows Server Failover Clustering shared storage in Azure services.</span></span>
* <span data-ttu-id="96390-120">Lär dig hur du kan skydda en punkt i fel komponenter som avancerade Business Application Programming (ABAP) SAP centrala tjänster (ASCS) / SAP centrala Services (SCS) och databashanteringssystem (DBMS) och redundanta komponenter som SAP Programserver i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-120">Learn how to help protect single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server, in Azure.</span></span>
* <span data-ttu-id="96390-121">Följ ett steg för steg-exempel på en installation och konfiguration av en SAP-systemet för hög tillgänglighet i ett redundanskluster i Windows Server-kluster i Azure med hjälp av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="96390-121">Follow a step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure by using Azure Resource Manager.</span></span>
* <span data-ttu-id="96390-122">Mer information om ytterligare steg som krävs för att använda Windows Server Failover Clustering i Azure, men som inte behövs i en lokal distribution.</span><span class="sxs-lookup"><span data-stu-id="96390-122">Learn about additional steps required to use Windows Server Failover Clustering in Azure, but which are not needed in an on-premises deployment.</span></span>

<span data-ttu-id="96390-123">Vi använder SAP tre skikt hög tillgänglighet Resource Manager-mallar för att förenkla distribution och konfiguration i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="96390-123">To simplify deployment and configuration, in this article, we use the SAP three-tier high-availability Resource Manager templates.</span></span> <span data-ttu-id="96390-124">Mallarna automatisera distributionen av hela infrastrukturen som du behöver för ett SAP-system med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="96390-124">The templates automate deployment of the entire infrastructure that you need for a high-availability SAP system.</span></span> <span data-ttu-id="96390-125">Infrastrukturen stöder också SAP programmets prestanda Standard (SAP) storleksändring av SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-125">The infrastructure also supports SAP Application Performance Standard (SAPS) sizing of your SAP system.</span></span>

## <span data-ttu-id="96390-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="96390-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Prerequisites</span></span>
<span data-ttu-id="96390-127">Innan du börjar bör du kontrollera att du uppfyller kraven som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="96390-127">Before you start, make sure that you meet the prerequisites that are described in the following sections.</span></span> <span data-ttu-id="96390-128">Dessutom måste du kontrollera att alla resurser som finns i den [resurser] [ sap-ha-guide-2] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="96390-128">Also, be sure to check all resources listed in the [Resources][sap-ha-guide-2] section.</span></span>

<span data-ttu-id="96390-129">I den här artikeln använder vi Azure Resource Manager-mallar för [trelagers-SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span><span class="sxs-lookup"><span data-stu-id="96390-129">In this article, we use Azure Resource Manager templates for [three-tier SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span></span> <span data-ttu-id="96390-130">En bra översikt över mallar finns [SAP Azure Resource Manager-mallar](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span><span class="sxs-lookup"><span data-stu-id="96390-130">For a helpful overview of templates, see [SAP Azure Resource Manager templates](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span></span>

## <span data-ttu-id="96390-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Resurser</span><span class="sxs-lookup"><span data-stu-id="96390-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources</span></span>
<span data-ttu-id="96390-132">Dessa artiklar beskriver SAP-distributioner i Azure:</span><span class="sxs-lookup"><span data-stu-id="96390-132">These articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="96390-133">[Azure virtuella datorer planering och implementering för SAP NetWeaver][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="96390-133">[Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide]</span></span>
* <span data-ttu-id="96390-134">[Distribution av Azure virtuella datorer för SAP NetWeaver][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="96390-134">[Azure Virtual Machines deployment for SAP NetWeaver][deployment-guide]</span></span>
* <span data-ttu-id="96390-135">[Azure virtuella datorer DBMS-distribution för SAP NetWeaver][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="96390-135">[Azure Virtual Machines DBMS deployment for SAP NetWeaver][dbms-guide]</span></span>
* <span data-ttu-id="96390-136">[Azure virtuella datorer hög tillgänglighet för SAP NetWeaver (den här guiden)][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="96390-136">[Azure Virtual Machines high availability for SAP NetWeaver (this guide)][sap-ha-guide]</span></span>

> [!NOTE]
> <span data-ttu-id="96390-137">När det är möjligt, får du en länk till den refererande SAP-installationsguiden (finns i [SAP installationen guider][sap-installation-guides]).</span><span class="sxs-lookup"><span data-stu-id="96390-137">Whenever possible, we give you a link to the referring SAP installation guide (see the [SAP installation guides][sap-installation-guides]).</span></span> <span data-ttu-id="96390-138">Det är en bra idé att läsa SAP NetWeaver installationen guider noggrant krav och information om installationen.</span><span class="sxs-lookup"><span data-stu-id="96390-138">For prerequisites and information about the installation process, it's a good idea to read the SAP NetWeaver installation guides carefully.</span></span> <span data-ttu-id="96390-139">Den här artikeln beskriver endast specifika uppgifter för SAP NetWeaver-baserade datorer som du kan använda med Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="96390-139">This article covers only specific tasks for SAP NetWeaver-based systems that you can use with Azure Virtual Machines.</span></span>
>
>

<span data-ttu-id="96390-140">Anteckningarna SAP är relaterade till SAP i Azure-avsnitt:</span><span class="sxs-lookup"><span data-stu-id="96390-140">These SAP Notes are related to the topic of SAP in Azure:</span></span>

| <span data-ttu-id="96390-141">Nummer</span><span class="sxs-lookup"><span data-stu-id="96390-141">Note number</span></span> | <span data-ttu-id="96390-142">Rubrik</span><span class="sxs-lookup"><span data-stu-id="96390-142">Title</span></span> |
| --- | --- |
| <span data-ttu-id="96390-143">[1928533]</span><span class="sxs-lookup"><span data-stu-id="96390-143">[1928533]</span></span> |<span data-ttu-id="96390-144">SAP-program i Azure: produkter och storlek som stöds</span><span class="sxs-lookup"><span data-stu-id="96390-144">SAP Applications on Azure: Supported Products and Sizing</span></span> |
| <span data-ttu-id="96390-145">[2015553]</span><span class="sxs-lookup"><span data-stu-id="96390-145">[2015553]</span></span> |<span data-ttu-id="96390-146">SAP på Microsoft Azure: stöd för krav</span><span class="sxs-lookup"><span data-stu-id="96390-146">SAP on Microsoft Azure: Support Prerequisites</span></span> |
| <span data-ttu-id="96390-147">[1999351]</span><span class="sxs-lookup"><span data-stu-id="96390-147">[1999351]</span></span> |<span data-ttu-id="96390-148">Förbättrad Azure övervakning för SAP</span><span class="sxs-lookup"><span data-stu-id="96390-148">Enhanced Azure Monitoring for SAP</span></span> |
| <span data-ttu-id="96390-149">[2178632]</span><span class="sxs-lookup"><span data-stu-id="96390-149">[2178632]</span></span> |<span data-ttu-id="96390-150">Key som övervakning mått för SAP på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="96390-150">Key Monitoring Metrics for SAP on Microsoft Azure</span></span> |
| <span data-ttu-id="96390-151">[1999351]</span><span class="sxs-lookup"><span data-stu-id="96390-151">[1999351]</span></span> |<span data-ttu-id="96390-152">Virtualisering på Windows: förbättrad övervakning</span><span class="sxs-lookup"><span data-stu-id="96390-152">Virtualization on Windows: Enhanced Monitoring</span></span> |
| <span data-ttu-id="96390-153">[2243692]</span><span class="sxs-lookup"><span data-stu-id="96390-153">[2243692]</span></span> |<span data-ttu-id="96390-154">Användning av Azure Premium SSD-lagring för SAP DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-154">Use of Azure Premium SSD Storage for SAP DBMS Instance</span></span> |

<span data-ttu-id="96390-155">Lär dig mer om den [begränsningar i Azure-prenumerationer][azure-subscription-service-limits-subscription], inklusive allmänna standardbegränsningar och maximala begränsningar.</span><span class="sxs-lookup"><span data-stu-id="96390-155">Learn more about the [limitations of Azure subscriptions][azure-subscription-service-limits-subscription], including general default limitations and maximum limitations.</span></span>

## <span data-ttu-id="96390-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Hög tillgänglighet SAP med Azure Resource Manager kontra Azure klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="96390-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>High-availability SAP with Azure Resource Manager vs. the Azure classic deployment model</span></span>
<span data-ttu-id="96390-157">Azure Resource Manager och Azure klassiska distributionsmodeller skiljer sig inom följande områden:</span><span class="sxs-lookup"><span data-stu-id="96390-157">The Azure Resource Manager and Azure classic deployment models are different in the following areas:</span></span>

- <span data-ttu-id="96390-158">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="96390-158">Resource groups</span></span>
- <span data-ttu-id="96390-159">Azure interna belastningen belastningsutjämnare beroende på Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="96390-159">Azure internal load balancer dependency on the Azure resource group</span></span>
- <span data-ttu-id="96390-160">Stöd för scenarier för SAP multi-SID</span><span class="sxs-lookup"><span data-stu-id="96390-160">Support for SAP multi-SID scenarios</span></span>

### <span data-ttu-id="96390-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="96390-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resource groups</span></span>
<span data-ttu-id="96390-162">Azure Resource Manager du använda resursgrupper för att hantera alla programresurser i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="96390-162">In Azure Resource Manager, you can use resource groups to manage all the application resources in your Azure subscription.</span></span> <span data-ttu-id="96390-163">En integrerad lösning i en resursgrupp, alla resurser som har samma livscykel.</span><span class="sxs-lookup"><span data-stu-id="96390-163">An integrated approach, in a resource group, all resources have the same life cycle.</span></span> <span data-ttu-id="96390-164">Till exempel alla resurser skapas på samma gång och de tas bort samtidigt.</span><span class="sxs-lookup"><span data-stu-id="96390-164">For example, all resources are created at the same time and they are deleted at the same time.</span></span> <span data-ttu-id="96390-165">Läs mer om [resursgrupper](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="96390-165">Learn more about [resource groups](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

### <span data-ttu-id="96390-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure interna belastningen belastningsutjämnare beroende på Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="96390-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure internal load balancer dependency on the Azure resource group</span></span>

<span data-ttu-id="96390-167">I Azure klassiska distributionsmodellen finns det ett beroende mellan Azure intern belastningsutjämnare (Azure belastningsutjämnare service) och gruppen cloud service.</span><span class="sxs-lookup"><span data-stu-id="96390-167">In the Azure classic deployment model, there is a dependency between the Azure internal load balancer (Azure Load Balancer service) and the cloud service group.</span></span> <span data-ttu-id="96390-168">Varje intern belastningsutjämnare måste en cloud service-grupp.</span><span class="sxs-lookup"><span data-stu-id="96390-168">Every internal load balancer needs one cloud service group.</span></span>

<span data-ttu-id="96390-169">Azure Resource Manager behöver du inte en Azure-resursgrupp att använda Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="96390-169">In Azure Resource Manager, you don't need an Azure resource group to use Azure Load Balancer.</span></span> <span data-ttu-id="96390-170">Miljön är enklare och mer flexibelt.</span><span class="sxs-lookup"><span data-stu-id="96390-170">The environment is simpler and more flexible.</span></span>

### <a name="support-for-sap-multi-sid-scenarios"></a><span data-ttu-id="96390-171">Stöd för scenarier för SAP multi-SID</span><span class="sxs-lookup"><span data-stu-id="96390-171">Support for SAP multi-SID scenarios</span></span>

<span data-ttu-id="96390-172">Du kan installera flera SAP system-ID (SID) ASCS/SCS instanser i ett kluster i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="96390-172">In Azure Resource Manager, you can install multiple SAP system identifier (SID) ASCS/SCS instances in one cluster.</span></span> <span data-ttu-id="96390-173">Multi-SID-instanserna är möjligt tack vare stöd för flera IP-adresser för varje Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="96390-173">Multi-SID instances are possible because of support for multiple IP addresses for each Azure internal load balancer.</span></span>

<span data-ttu-id="96390-174">Om du vill använda Azure klassiska distributionsmodellen, följer du procedurerna som beskrivs i [SAP NetWeaver i Azure: Clustering SAP ASCS/SCS instanser med hjälp av Windows Server Failover Clustering i Azure med SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span><span class="sxs-lookup"><span data-stu-id="96390-174">To use the Azure classic deployment model, follow the procedures described in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96390-175">Vi rekommenderar starkt att du använder Azure Resource Manager-distributionsmodellen för SAP-installationer.</span><span class="sxs-lookup"><span data-stu-id="96390-175">We strongly recommend that you use the Azure Resource Manager deployment model for your SAP installations.</span></span> <span data-ttu-id="96390-176">Den erbjuder många fördelar som inte är tillgängliga i den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="96390-176">It offers many benefits that are not available in the classic deployment model.</span></span> <span data-ttu-id="96390-177">Lär dig mer om Azure [distributionsmodeller][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span><span class="sxs-lookup"><span data-stu-id="96390-177">Learn more about Azure [deployment models][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span></span>   
>
>

## <span data-ttu-id="96390-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover-kluster</span><span class="sxs-lookup"><span data-stu-id="96390-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Failover Clustering</span></span>
<span data-ttu-id="96390-179">Windows Server Failover Clustering är grunden för en hög tillgänglighet SAP ASCS/SCS installation och DBMS i Windows.</span><span class="sxs-lookup"><span data-stu-id="96390-179">Windows Server Failover Clustering is the foundation of a high-availability SAP ASCS/SCS installation and DBMS in Windows.</span></span>

<span data-ttu-id="96390-180">Ett redundanskluster är en grupp 1 + n oberoende servrar (noder) som arbetar tillsammans för att öka tillgängligheten för program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="96390-180">A failover cluster is a group of 1+n independent servers (nodes) that work together to increase the availability of applications and services.</span></span> <span data-ttu-id="96390-181">Om det inträffar ett nodfel, beräknar Windows Server Failover Clustering antalet fel som kan uppstå samtidigt som ett felfritt kluster för att tillhandahålla program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="96390-181">If a node failure occurs, Windows Server Failover Clustering calculates the number of failures that can occur while maintaining a healthy cluster to provide applications and services.</span></span> <span data-ttu-id="96390-182">Du kan välja från olika kvorumlägen att uppnå failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="96390-182">You can choose from different quorum modes to achieve failover clustering.</span></span>

### <span data-ttu-id="96390-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Kvorumlägen</span><span class="sxs-lookup"><span data-stu-id="96390-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum modes</span></span>
<span data-ttu-id="96390-184">Du kan välja mellan fyra kvorumlägen när du använder Windows Server Failover Clustering:</span><span class="sxs-lookup"><span data-stu-id="96390-184">You can choose from four quorum modes when you use Windows Server Failover Clustering:</span></span>

* <span data-ttu-id="96390-185">**Nodmajoritet**.</span><span class="sxs-lookup"><span data-stu-id="96390-185">**Node Majority**.</span></span> <span data-ttu-id="96390-186">Varje nod i klustret kan rösta.</span><span class="sxs-lookup"><span data-stu-id="96390-186">Each node of the cluster can vote.</span></span> <span data-ttu-id="96390-187">Klustret fungerar bara med en majoritet av röster, det vill säga med mer än hälften röster.</span><span class="sxs-lookup"><span data-stu-id="96390-187">The cluster functions only with a majority of votes, that is, with more than half the votes.</span></span> <span data-ttu-id="96390-188">Vi rekommenderar att det här alternativet för kluster som har ett ojämnt antal noder.</span><span class="sxs-lookup"><span data-stu-id="96390-188">We recommend this option for clusters that have an uneven number of nodes.</span></span> <span data-ttu-id="96390-189">Till exempel tre noder i ett kluster med sju noder kan misslyckas och klustret stillbilder uppnår en majoritet och fortsätter att köras.</span><span class="sxs-lookup"><span data-stu-id="96390-189">For example, three nodes in a seven-node cluster can fail, and the cluster stills achieves a majority and continues to run.</span></span>  
* <span data-ttu-id="96390-190">**Nod- och**.</span><span class="sxs-lookup"><span data-stu-id="96390-190">**Node and Disk Majority**.</span></span> <span data-ttu-id="96390-191">Varje nod och en angiven disk (ett diskvittne) i klusterlagringen kan rösta när de är tillgängliga och kommunikation.</span><span class="sxs-lookup"><span data-stu-id="96390-191">Each node and a designated disk (a disk witness) in the cluster storage can vote when they are available and in communication.</span></span> <span data-ttu-id="96390-192">Klustret fungerar bara med en majoritet av röster, det vill säga med mer än hälften röster.</span><span class="sxs-lookup"><span data-stu-id="96390-192">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="96390-193">Det här läget är meningsfullt i en klustermiljö med ett jämnt antal noder.</span><span class="sxs-lookup"><span data-stu-id="96390-193">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="96390-194">Om hälften av noderna och disken är online kan förblir klustret i felfritt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="96390-194">If half the nodes and the disk are online, the cluster remains in a healthy state.</span></span>
* <span data-ttu-id="96390-195">**Nod- och filresurs majoritet**.</span><span class="sxs-lookup"><span data-stu-id="96390-195">**Node and File Share Majority**.</span></span> <span data-ttu-id="96390-196">Varje nod plus en avsedda filresurs (ett filresursvittne) som skapar administratören kan rösta, oavsett om noderna och filresursen är tillgänglig och kommunikation.</span><span class="sxs-lookup"><span data-stu-id="96390-196">Each node plus a designated file share (a file share witness) that the administrator creates can vote, regardless of whether the nodes and file share are available and in communication.</span></span> <span data-ttu-id="96390-197">Klustret fungerar bara med en majoritet av röster, det vill säga med mer än hälften röster.</span><span class="sxs-lookup"><span data-stu-id="96390-197">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="96390-198">Det här läget är meningsfullt i en klustermiljö med ett jämnt antal noder.</span><span class="sxs-lookup"><span data-stu-id="96390-198">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="96390-199">Den liknar nod- och diskmajoritet läge, men den använder en filresurs vittne i stället för en vittnesdisk.</span><span class="sxs-lookup"><span data-stu-id="96390-199">It's similar to the Node and Disk Majority mode, but it uses a witness file share instead of a witness disk.</span></span> <span data-ttu-id="96390-200">Det här läget är enkel att implementera, men om filresursen själva har inte hög tillgänglighet, så kan det bli en enskild felpunkt.</span><span class="sxs-lookup"><span data-stu-id="96390-200">This mode is easy to implement, but if the file share itself is not highly available, it might become a single point of failure.</span></span>
* <span data-ttu-id="96390-201">**Ingen majoritet: Endast Disk**.</span><span class="sxs-lookup"><span data-stu-id="96390-201">**No Majority: Disk Only**.</span></span> <span data-ttu-id="96390-202">Klustret har ett kvorum om en nod är tillgänglig och kommunikation med en viss disk i klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="96390-202">The cluster has a quorum if one node is available and in communication with a specific disk in the cluster storage.</span></span> <span data-ttu-id="96390-203">Endast de noder som är också vid kommunikation med disken kan ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-203">Only the nodes that also are in communication with that disk can join the cluster.</span></span> <span data-ttu-id="96390-204">Vi rekommenderar att du inte använder det här läget.</span><span class="sxs-lookup"><span data-stu-id="96390-204">We recommend that you do not use this mode.</span></span>
 

## <span data-ttu-id="96390-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server Failover-kluster lokalt</span><span class="sxs-lookup"><span data-stu-id="96390-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Server Failover Clustering on-premises</span></span>
<span data-ttu-id="96390-206">Bild 1 visar ett kluster med två noder.</span><span class="sxs-lookup"><span data-stu-id="96390-206">Figure 1 shows a cluster of two nodes.</span></span> <span data-ttu-id="96390-207">Om nätverksanslutningen mellan noder misslyckas och både noder stanna upp och köras, en kvorumdisk eller fil delar avgör vilken nod som kommer att fortsätta att tillhandahålla klustrets program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="96390-207">If the network connection between the nodes fails and both nodes stay up and running, a quorum disk or file share determines which node will continue to provide the cluster's applications and services.</span></span> <span data-ttu-id="96390-208">Noden som har åtkomst till kvorum disk eller filresurs är den nod som säkerställer att tjänster fortsätter.</span><span class="sxs-lookup"><span data-stu-id="96390-208">The node that has access to the quorum disk or file share is the node that ensures that services continue.</span></span>

<span data-ttu-id="96390-209">Eftersom det här exemplet använder ett kluster med två noder kan använda vi kvorumläge för nod och filresursmajoritet.</span><span class="sxs-lookup"><span data-stu-id="96390-209">Because this example uses a two-node cluster, we use the Node and File Share Majority quorum mode.</span></span> <span data-ttu-id="96390-210">Nod- och diskmajoritet är också ett giltigt alternativ.</span><span class="sxs-lookup"><span data-stu-id="96390-210">The Node and Disk Majority also is a valid option.</span></span> <span data-ttu-id="96390-211">I en produktionsmiljö rekommenderar vi att du använder en kvorumdisk.</span><span class="sxs-lookup"><span data-stu-id="96390-211">In a production environment, we recommend that you use a quorum disk.</span></span> <span data-ttu-id="96390-212">Nätverks- och system-tekniken kan användas för att ge den hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="96390-212">You can use network and storage system technology to make it highly available.</span></span>

![Bild 1: Exempel på en Windows Server Failover Clustering konfiguration för SAP ASCS/SCS i Azure][sap-ha-guide-figure-1000]

<span data-ttu-id="96390-214">_**Bild 1:** exempel på en Windows Server Failover Clustering konfiguration för SAP ASCS/SCS i Azure_</span><span class="sxs-lookup"><span data-stu-id="96390-214">_**Figure 1:** Example of a Windows Server Failover Clustering configuration for SAP ASCS/SCS in Azure_</span></span>

### <span data-ttu-id="96390-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Delad lagring</span><span class="sxs-lookup"><span data-stu-id="96390-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Shared storage</span></span>
<span data-ttu-id="96390-216">Bild 1 visar också ett kluster med två noder delad lagring.</span><span class="sxs-lookup"><span data-stu-id="96390-216">Figure 1 also shows a two-node shared storage cluster.</span></span> <span data-ttu-id="96390-217">I ett kluster för lokala delad lagring identifiera alla noder i klustret delad lagring.</span><span class="sxs-lookup"><span data-stu-id="96390-217">In an on-premises shared storage cluster, all nodes in the cluster detect shared storage.</span></span> <span data-ttu-id="96390-218">En mekanism för låsning förhindrar skadade data.</span><span class="sxs-lookup"><span data-stu-id="96390-218">A locking mechanism protects the data from corruption.</span></span> <span data-ttu-id="96390-219">Alla noder kan känna av om en annan nod misslyckas.</span><span class="sxs-lookup"><span data-stu-id="96390-219">All nodes can detect if another node fails.</span></span> <span data-ttu-id="96390-220">Om en nod misslyckas den återstående noden ägarskapet till storage-resurser och garanterar tillgängligheten av tjänster.</span><span class="sxs-lookup"><span data-stu-id="96390-220">If one node fails, the remaining node takes ownership of the storage resources and ensures the availability of services.</span></span>

> [!NOTE]
> <span data-ttu-id="96390-221">Du behöver inte delade diskar för hög tillgänglighet med vissa DBMS-program, precis som med SQL Server.</span><span class="sxs-lookup"><span data-stu-id="96390-221">You don't need shared disks for high availability with some DBMS applications, like with SQL Server.</span></span> <span data-ttu-id="96390-222">SQL Server alltid aktiverad replikerar du DBMS data och loggfilen filerna från den lokala disken på en klusternod till den lokala disken på en annan klusternod.</span><span class="sxs-lookup"><span data-stu-id="96390-222">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="96390-223">I så fall behöver klusterkonfigurationen Windows inte en delad disk.</span><span class="sxs-lookup"><span data-stu-id="96390-223">In that case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="96390-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Nätverk och namnmatchning</span><span class="sxs-lookup"><span data-stu-id="96390-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Networking and name resolution</span></span>
<span data-ttu-id="96390-225">Klientdatorer når klustret via en virtuell IP-adress och ett virtuellt värdnamn som ger DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="96390-225">Client computers reach the cluster over a virtual IP address and a virtual host name that the DNS server provides.</span></span> <span data-ttu-id="96390-226">Lokala noder och DNS-server kan hantera flera IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="96390-226">The on-premises nodes and the DNS server can handle multiple IP addresses.</span></span>

<span data-ttu-id="96390-227">I en typisk konfiguration kan du använda två eller flera nätverksanslutningar:</span><span class="sxs-lookup"><span data-stu-id="96390-227">In a typical setup, you use two or more network connections:</span></span>

* <span data-ttu-id="96390-228">Dedicerad anslutning till lagring</span><span class="sxs-lookup"><span data-stu-id="96390-228">A dedicated connection to the storage</span></span>
* <span data-ttu-id="96390-229">En kluster-interna nätverksanslutning för pulsslag</span><span class="sxs-lookup"><span data-stu-id="96390-229">A cluster-internal network connection for the heartbeat</span></span>
* <span data-ttu-id="96390-230">Ett offentligt nätverk som klienter använder för att ansluta till klustret</span><span class="sxs-lookup"><span data-stu-id="96390-230">A public network that clients use to connect to the cluster</span></span>

## <span data-ttu-id="96390-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="96390-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="96390-232">Azure Virtual Machines krävs jämfört med bare metal eller privat molndistributioner, ytterligare steg för att konfigurera Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="96390-232">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="96390-233">När du skapar en delad klusterdisk som du behöver konfigurera flera IP-adresser och virtuella värdnamn för SAP ASCS/SCS-instansen.</span><span class="sxs-lookup"><span data-stu-id="96390-233">When you build a shared cluster disk, you need to set several IP addresses and virtual host names for the SAP ASCS/SCS instance.</span></span>

<span data-ttu-id="96390-234">I den här artikeln tar vi upp viktiga begrepp och ytterligare steg som krävs för att skapa ett kluster med SAP centrala tjänster med hög tillgänglighet i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-234">In this article, we discuss key concepts and the additional steps required to build an SAP high-availability central services cluster in Azure.</span></span> <span data-ttu-id="96390-235">Vi visar dig hur du ställer in tredjepartsverktyg SIOS DataKeeper och hur du konfigurerar Azure intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="96390-235">We show you how to set up the third-party tool SIOS DataKeeper, and how to configure the Azure internal load balancer.</span></span> <span data-ttu-id="96390-236">Du kan använda dessa verktyg för att skapa ett redundanskluster i Windows med ett filresursvittne i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-236">You can use these tools to create a Windows failover cluster with a file share witness in Azure.</span></span>

![Bild 2: Windows Server Failover Clustering konfiguration i Azure utan en delad disk][sap-ha-guide-figure-1001]

<span data-ttu-id="96390-238">_**Bild 2:** Windows Server Failover Clustering konfiguration i Azure utan en delad disk_</span><span class="sxs-lookup"><span data-stu-id="96390-238">_**Figure 2:** Windows Server Failover Clustering configuration in Azure without a shared disk_</span></span>

### <span data-ttu-id="96390-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Delad disk i Azure med SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="96390-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Shared disk in Azure with SIOS DataKeeper</span></span>
<span data-ttu-id="96390-240">Du måste ingå i klustret delad lagring för en SAP ASCS/SCS-instans med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="96390-240">You need cluster shared storage for a high-availability SAP ASCS/SCS instance.</span></span> <span data-ttu-id="96390-241">Från och med September 2016 erbjuder inte Azure delad lagring som du kan använda för att skapa ett kluster med delad lagring.</span><span class="sxs-lookup"><span data-stu-id="96390-241">As of September 2016, Azure doesn't offer shared storage that you can use to create a shared storage cluster.</span></span> <span data-ttu-id="96390-242">Du kan använda tredjepartsprogram SIOS DataKeeper Cluster Edition för att skapa en speglad lagring som simulerar delad klusterlagring.</span><span class="sxs-lookup"><span data-stu-id="96390-242">You can use third-party software SIOS DataKeeper Cluster Edition to create a mirrored storage that simulates cluster shared storage.</span></span> <span data-ttu-id="96390-243">SIOS lösningen ger realtidsdata synkron replikering.</span><span class="sxs-lookup"><span data-stu-id="96390-243">The SIOS solution provides real-time synchronous data replication.</span></span> <span data-ttu-id="96390-244">Detta beror på hur du kan skapa en delad diskresurs för ett kluster:</span><span class="sxs-lookup"><span data-stu-id="96390-244">This is how you can create a shared disk resource for a cluster:</span></span>

1. <span data-ttu-id="96390-245">Koppla en ytterligare Azure virtuell hårddisk (VHD) till var och en av de virtuella datorerna (VM) i en Windows-klusterkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="96390-245">Attach an additional Azure virtual hard disk (VHD) to each of the virtual machines (VMs) in a Windows cluster configuration.</span></span>
2. <span data-ttu-id="96390-246">Kör SIOS DataKeeper Cluster Edition på båda noderna för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="96390-246">Run SIOS DataKeeper Cluster Edition on both virtual machine nodes.</span></span>
3. <span data-ttu-id="96390-247">Konfigurera SIOS DataKeeper Cluster Edition så att den speglar innehållet i ytterligare virtuell Hårddisk ansluten-volym från den virtuella källdatorn med ytterligare virtuell Hårddisk ansluten volymen av den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="96390-247">Configure SIOS DataKeeper Cluster Edition so that it mirrors the content of the additional VHD attached volume from the source virtual machine to the additional VHD attached volume of the target virtual machine.</span></span> <span data-ttu-id="96390-248">SIOS DataKeeper avlägsnar käll- och lokala volymer och visar dem till Windows Server Failover Clustering som en delad disk.</span><span class="sxs-lookup"><span data-stu-id="96390-248">SIOS DataKeeper abstracts the source and target local volumes, and then presents them to Windows Server Failover Clustering as one shared disk.</span></span>

<span data-ttu-id="96390-249">Hämta mer information [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span><span class="sxs-lookup"><span data-stu-id="96390-249">Get more information about [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span></span>

![Bild 3: Windows Server Failover Clustering konfigurationen i Azure med SIOS DataKeeper][sap-ha-guide-figure-1002]

<span data-ttu-id="96390-251">_**Bild 3:** Windows Server Failover Clustering konfigurationen i Azure med SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="96390-251">_**Figure 3:** Windows Server Failover Clustering configuration in Azure with SIOS DataKeeper_</span></span>

> [!NOTE]
> <span data-ttu-id="96390-252">Du behöver inte delade diskar för hög tillgänglighet med vissa DBMS-produkter, t.ex. SQL Server.</span><span class="sxs-lookup"><span data-stu-id="96390-252">You don't need shared disks for high availability with some DBMS products, like SQL Server.</span></span> <span data-ttu-id="96390-253">SQL Server alltid aktiverad replikerar du DBMS data och loggfilen filerna från den lokala disken på en klusternod till den lokala disken på en annan klusternod.</span><span class="sxs-lookup"><span data-stu-id="96390-253">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="96390-254">I det här fallet behöver klusterkonfigurationen Windows inte en delad disk.</span><span class="sxs-lookup"><span data-stu-id="96390-254">In this case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="96390-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Namnmatchning i Azure</span><span class="sxs-lookup"><span data-stu-id="96390-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Name resolution in Azure</span></span>
<span data-ttu-id="96390-256">Plattformen Azure-molnet kan inte välja att konfigurera den virtuella IP-adresser som flytande IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="96390-256">The Azure cloud platform doesn't offer the option to configure virtual IP addresses, such as floating IP addresses.</span></span> <span data-ttu-id="96390-257">Du måste en alternativ lösning för att konfigurera en virtuell IP-adress till klusterresursen i molnet.</span><span class="sxs-lookup"><span data-stu-id="96390-257">You need an alternative solution to set up a virtual IP address to reach the cluster resource in the cloud.</span></span>
<span data-ttu-id="96390-258">Azure har en intern belastningsutjämnare i tjänsten Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="96390-258">Azure has an internal load balancer in the Azure Load Balancer service.</span></span> <span data-ttu-id="96390-259">Med den interna belastningsutjämnaren nå klienter klustret via klustrets virtuella IP-adress.</span><span class="sxs-lookup"><span data-stu-id="96390-259">With the internal load balancer, clients reach the cluster over the cluster virtual IP address.</span></span>
<span data-ttu-id="96390-260">Du måste distribuera den interna belastningsutjämnaren i resursgruppen som innehåller klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="96390-260">You need to deploy the internal load balancer in the resource group that contains the cluster nodes.</span></span> <span data-ttu-id="96390-261">Konfigurera sedan alla nödvändiga port vidarebefordran regler med avsökningen portar för den interna belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="96390-261">Then, configure all necessary port forwarding rules with the probe ports of the internal load balancer.</span></span>
<span data-ttu-id="96390-262">Klienterna kan ansluta via virtuella värdnamn.</span><span class="sxs-lookup"><span data-stu-id="96390-262">The clients can connect via the virtual host name.</span></span> <span data-ttu-id="96390-263">DNS-servern löser klustrets IP-adress och interna belastningsutjämnare handtag porten vidarebefordran till den aktiva noden i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-263">The DNS server resolves the cluster IP address, and the internal load balancer handles port forwarding to the active node of the cluster.</span></span>

## <span data-ttu-id="96390-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver hög tillgänglighet i Azure Infrastructure-as-a-Service (IaaS)</span><span class="sxs-lookup"><span data-stu-id="96390-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver high availability in Azure Infrastructure-as-a-Service (IaaS)</span></span>
<span data-ttu-id="96390-265">För att uppnå SAP-program med hög tillgänglighet som för SAP programvarukomponenter, måste du skydda följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="96390-265">To achieve SAP application high availability, such as for SAP software components, you need to protect the following components:</span></span>

* <span data-ttu-id="96390-266">SAP Application Server-instans</span><span class="sxs-lookup"><span data-stu-id="96390-266">SAP Application Server instance</span></span>
* <span data-ttu-id="96390-267">SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-267">SAP ASCS/SCS instance</span></span>
* <span data-ttu-id="96390-268">DBMS-server</span><span class="sxs-lookup"><span data-stu-id="96390-268">DBMS server</span></span>

<span data-ttu-id="96390-269">Mer information om hur du skyddar SAP komponenter i scenarier med hög tillgänglighet finns [Azure Virtual Machines planering och implementering för SAP NetWeaver](planning-guide.md).</span><span class="sxs-lookup"><span data-stu-id="96390-269">For more information about protecting SAP components in high-availability scenarios, see [Azure Virtual Machines planning and implementation for SAP NetWeaver](planning-guide.md).</span></span>

### <span data-ttu-id="96390-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Hög tillgänglighet SAP-programserver</span><span class="sxs-lookup"><span data-stu-id="96390-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> High-availability SAP Application Server</span></span>
<span data-ttu-id="96390-271">Vanligtvis behöver du inte en specifik lösning för hög tillgänglighet för SAP-programserver och dialogrutan instanser.</span><span class="sxs-lookup"><span data-stu-id="96390-271">You usually don't need a specific high-availability solution for the SAP Application Server and dialog instances.</span></span> <span data-ttu-id="96390-272">Du kan uppnå hög tillgänglighet med redundans och konfigurerar du dialogrutan alla instanser i olika instanser av virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-272">You achieve high availability by redundancy, and you'll configure multiple dialog instances in different instances of Azure Virtual Machines.</span></span> <span data-ttu-id="96390-273">Du bör ha minst två instanser för SAP-program installeras i två instanser av virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-273">You should have at least two SAP application instances installed in two instances of Azure Virtual Machines.</span></span>

![Bild 4: Hög tillgänglighet SAP-programserver][sap-ha-guide-figure-2000]

<span data-ttu-id="96390-275">_**Bild 4:** hög tillgänglighet SAP-programserver_</span><span class="sxs-lookup"><span data-stu-id="96390-275">_**Figure 4:** High-availability SAP Application Server_</span></span>

<span data-ttu-id="96390-276">Du måste placera alla virtuella datorer som värd SAP Application Server-instanser i samma Azure tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="96390-276">You must place all virtual machines that host SAP Application Server instances in the same Azure availability set.</span></span> <span data-ttu-id="96390-277">En Azure tillgänglighetsuppsättning säkerställer att:</span><span class="sxs-lookup"><span data-stu-id="96390-277">An Azure availability set ensures that:</span></span>

* <span data-ttu-id="96390-278">Alla virtuella datorer är en del av samma uppgraderingsdomänen.</span><span class="sxs-lookup"><span data-stu-id="96390-278">All virtual machines are part of the same upgrade domain.</span></span> <span data-ttu-id="96390-279">En uppgraderingsdomän, till exempel ser till att de virtuella datorerna inte är uppdaterade på samma gång under planerat underhåll driftstopp.</span><span class="sxs-lookup"><span data-stu-id="96390-279">An upgrade domain, for example, makes sure that the virtual machines aren't updated at the same time during planned maintenance downtime.</span></span>
* <span data-ttu-id="96390-280">Alla virtuella datorer ingår i samma feldomän.</span><span class="sxs-lookup"><span data-stu-id="96390-280">All virtual machines are part of the same fault domain.</span></span> <span data-ttu-id="96390-281">En feldomän, till exempel ser till att virtuella datorer distribueras så att ingen enskild felpunkt påverkar tillgängligheten för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-281">A fault domain, for example, makes sure that virtual machines are deployed so that no single point of failure affects the availability of all virtual machines.</span></span>

<span data-ttu-id="96390-282">Mer information om hur du [hantera tillgängligheten för virtuella datorer][virtual-machines-manage-availability].</span><span class="sxs-lookup"><span data-stu-id="96390-282">Learn more about how to [manage the availability of virtual machines][virtual-machines-manage-availability].</span></span>

<span data-ttu-id="96390-283">Eftersom Azure storage-konto är en potentiell felpunkt, är det viktigt att ha minst två Azure storage-konton som minst två virtuella datorer distribueras.</span><span class="sxs-lookup"><span data-stu-id="96390-283">Because the Azure storage account is a potential single point of failure, it's important to have at least two Azure storage accounts, in which at least two virtual machines are distributed.</span></span> <span data-ttu-id="96390-284">I en perfekt installationsprogrammet skulle diskar för varje virtuell dator som kör en instans för SAP-dialogrutan distribueras i ett annat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="96390-284">In an ideal setup, the disks of each virtual machine that is running an SAP dialog instance would be deployed in a different storage account.</span></span>

### <span data-ttu-id="96390-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Hög tillgänglighet SAP ASCS/SCS instans</span><span class="sxs-lookup"><span data-stu-id="96390-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> High-availability SAP ASCS/SCS instance</span></span>
<span data-ttu-id="96390-286">Bild 5 är ett exempel på en hög tillgänglighet SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-286">Figure 5 is an example of a high-availability SAP ASCS/SCS instance.</span></span>

![Bild 5: Hög tillgänglighet SAP ASCS/SCS instans][sap-ha-guide-figure-2001]

<span data-ttu-id="96390-288">_**Bild 5:** hög tillgänglighet SAP ASCS/SCS-instans_</span><span class="sxs-lookup"><span data-stu-id="96390-288">_**Figure 5:** High-availability SAP ASCS/SCS instance_</span></span>

#### <span data-ttu-id="96390-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASCS/SCS instans hög tillgänglighet med Windows Server Failover Clustering i Azure</span><span class="sxs-lookup"><span data-stu-id="96390-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS instance high availability with Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="96390-290">Azure Virtual Machines krävs jämfört med bare metal eller privat molndistributioner, ytterligare steg för att konfigurera Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="96390-290">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="96390-291">För att skapa ett redundanskluster i Windows, behöver du en delad klusterdisk, flera IP-adresser, flera virtuella värdnamn och en Azure intern belastningsutjämnare för kluster för en SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-291">To build a Windows failover cluster, you need a shared cluster disk, several IP addresses, several virtual host names, and an Azure internal load balancer for clustering an SAP ASCS/SCS instance.</span></span> <span data-ttu-id="96390-292">Detta diskuterar vi i detalj senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="96390-292">We discuss this in more detail later in the article.</span></span>

![Bild 6: Windows Server Failover Clustering för en SAP ASCS/SCS-konfiguration i Azure med hjälp av SIOS DataKeeper][sap-ha-guide-figure-1002]

<span data-ttu-id="96390-294">_**Bild 6:** Windows Server Failover Clustering för en SAP ASCS/SCS-konfiguration i Azure med SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="96390-294">_**Figure 6:** Windows Server Failover Clustering for an SAP ASCS/SCS configuration in Azure with SIOS DataKeeper_</span></span>

### <span data-ttu-id="96390-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Hög tillgänglighet DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>High-availability DBMS instance</span></span>
<span data-ttu-id="96390-296">DBMS är också en enda kontaktpunkt i ett SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-296">The DBMS also is a single point of contact in an SAP system.</span></span> <span data-ttu-id="96390-297">Du måste skydda den med hjälp av en lösning för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="96390-297">You need to protect it by using a high-availability solution.</span></span> <span data-ttu-id="96390-298">Bild 7 visar en SQL Server alltid på hög tillgänglighet lösning i Azure, med Windows Server Failover Clustering och Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="96390-298">Figure 7 shows a SQL Server Always On high-availability solution in Azure, with Windows Server Failover Clustering and the Azure internal load balancer.</span></span> <span data-ttu-id="96390-299">SQL Server alltid aktiverad replikerar DBMS data och loggfilen filer med hjälp av sin egen DBMS-replikering.</span><span class="sxs-lookup"><span data-stu-id="96390-299">SQL Server Always On replicates DBMS data and log files by using its own DBMS replication.</span></span> <span data-ttu-id="96390-300">I detta fall kan behöver du inte kluster delade diskar som förenklar hela installationen.</span><span class="sxs-lookup"><span data-stu-id="96390-300">In this case, you don't need cluster shared disks, which simplifies the entire setup.</span></span>

![Bild 7: Exempel på en SAP-DBMS med hög tillgänglighet, med SQL Server alltid aktiverad][sap-ha-guide-figure-2003]

<span data-ttu-id="96390-302">_**Bild 7:** exempel på ett SAP-DBMS med hög tillgänglighet, med SQL Server alltid aktiverad_</span><span class="sxs-lookup"><span data-stu-id="96390-302">_**Figure 7:** Example of a high-availability SAP DBMS, with SQL Server Always On_</span></span>

<span data-ttu-id="96390-303">Mer information om kluster och SQL Server i Azure med hjälp av Azure Resource Manager-distributionsmodellen finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="96390-303">For more information about clustering SQL Server in Azure by using the Azure Resource Manager deployment model, see these articles:</span></span>

* <span data-ttu-id="96390-304">[Konfigurera Always On-tillgänglighetsgrupp i Azure Virtual Machines manuellt med hjälp av hanteraren för filserverresurser][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span><span class="sxs-lookup"><span data-stu-id="96390-304">[Configure Always On availability group in Azure Virtual Machines manually by using Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span></span>
* <span data-ttu-id="96390-305">[Konfigurera en Azure intern belastningsutjämnare för en tillgänglighetsgrupp alltid på i Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span><span class="sxs-lookup"><span data-stu-id="96390-305">[Configure an Azure internal load balancer for an Always On availability group in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span></span>

## <span data-ttu-id="96390-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Distributionsscenarier för slutpunkt till slutpunkt hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="96390-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end high-availability deployment scenarios</span></span>

### <a name="deployment-scenario-using-architectural-template-1"></a><span data-ttu-id="96390-307">Scenario för distribution med arkitekturen mall 1</span><span class="sxs-lookup"><span data-stu-id="96390-307">Deployment scenario using Architectural Template 1</span></span>

<span data-ttu-id="96390-308">Bild 8 visar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **en** SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-308">Figure 8 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="96390-309">Det här scenariot är inställd på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="96390-309">This scenario is set up as follows:</span></span>

- <span data-ttu-id="96390-310">Ett dedikerat kluster används för SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-310">One dedicated cluster is used for the SAP ASCS/SCS instance.</span></span>
- <span data-ttu-id="96390-311">Ett dedikerat kluster används för DBMS-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-311">One dedicated cluster is used for the DBMS instance.</span></span>
- <span data-ttu-id="96390-312">SAP Application Server-instanser har distribuerats i sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-312">SAP Application Server instances are deployed in their own dedicated VMs.</span></span>

![Bild 8: SAP hög tillgänglighet arkitektur mall 1, med dedikerade kluster för ASCS/SCS och DBMS][sap-ha-guide-figure-2004]

<span data-ttu-id="96390-314">_**Bild 8:** SAP hög tillgänglighet arkitektur mall 1 dedikerade kluster för ASCS/SCS och DBMS_</span><span class="sxs-lookup"><span data-stu-id="96390-314">_**Figure 8:** SAP high-availability Architectural Template 1, dedicated clusters for ASCS/SCS and DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-2"></a><span data-ttu-id="96390-315">Scenario för distribution med arkitekturen mall 2</span><span class="sxs-lookup"><span data-stu-id="96390-315">Deployment scenario using Architectural Template 2</span></span>

<span data-ttu-id="96390-316">Bild 9 illustrerar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **en** SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-316">Figure 9 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="96390-317">Det här scenariot är inställd på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="96390-317">This scenario is set up as follows:</span></span>

- <span data-ttu-id="96390-318">Ett dedikerat kluster används för **både** SAP ASCS/SCS-instans och DBMS.</span><span class="sxs-lookup"><span data-stu-id="96390-318">One dedicated cluster is used for **both** the SAP ASCS/SCS instance and the DBMS.</span></span>
- <span data-ttu-id="96390-319">SAP Application Server-instanser har distribuerats i egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-319">SAP Application Server instances are deployed in own dedicated VMs.</span></span>

![Bild 9: SAP hög tillgänglighet arkitektur mall 2, med ett kluster för ASCS/SCS och ett kluster för DBMS][sap-ha-guide-figure-2005]

<span data-ttu-id="96390-321">_**Bild 9:** SAP hög tillgänglighet arkitektur mall 2, med ett kluster för ASCS/SCS och ett kluster för DBMS_</span><span class="sxs-lookup"><span data-stu-id="96390-321">_**Figure 9:** SAP high-availability Architectural Template 2, with a dedicated cluster for ASCS/SCS and a dedicated cluster for DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-3"></a><span data-ttu-id="96390-322">Scenario för distribution med arkitekturen mall 3</span><span class="sxs-lookup"><span data-stu-id="96390-322">Deployment scenario using Architectural Template 3</span></span>

<span data-ttu-id="96390-323">Bild 10 illustrerar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **två** SAP-system med &lt;SID1&gt; och &lt;SID2&gt;.</span><span class="sxs-lookup"><span data-stu-id="96390-323">Figure 10 shows an example of an SAP NetWeaver high-availability architecture in Azure for **two** SAP systems, with &lt;SID1&gt; and &lt;SID2&gt;.</span></span> <span data-ttu-id="96390-324">Det här scenariot är inställd på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="96390-324">This scenario is set up as follows:</span></span>

- <span data-ttu-id="96390-325">Ett dedikerat kluster används för **både** SAP ASCS/SCS SID1 instansen *och* SAP ASCS/SCS SID2-instans (kluster).</span><span class="sxs-lookup"><span data-stu-id="96390-325">One dedicated cluster is used for **both** the SAP ASCS/SCS SID1 instance *and* the SAP ASCS/SCS SID2 instance (one cluster).</span></span>
- <span data-ttu-id="96390-326">Ett dedikerat kluster används för DBMS SID1 och ett annat dedikerade kluster används för DBMS SID2 (två kluster).</span><span class="sxs-lookup"><span data-stu-id="96390-326">One dedicated cluster is used for DBMS SID1, and another dedicated cluster is used for DBMS SID2 (two clusters).</span></span>
- <span data-ttu-id="96390-327">SAP Application Server-instanser för SAP-systemet SID1 har sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-327">SAP Application Server instances for the SAP system SID1 have their own dedicated VMs.</span></span>
- <span data-ttu-id="96390-328">SAP Application Server-instanser för SAP-systemet SID2 har sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-328">SAP Application Server instances for the SAP system SID2 have their own dedicated VMs.</span></span>

![Figur 10: SAP hög tillgänglighet arkitektur mall 3, med ett kluster för olika ASCS/SCS-instanser][sap-ha-guide-figure-6003]

<span data-ttu-id="96390-330">_**Figur 10:** SAP hög tillgänglighet arkitektur mall 3, med ett kluster för olika ASCS/SCS-instanser_</span><span class="sxs-lookup"><span data-stu-id="96390-330">_**Figure 10:** SAP high-availability Architectural Template 3, with a dedicated cluster for different ASCS/SCS instances_</span></span>

## <span data-ttu-id="96390-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Förbered infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="96390-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Prepare the infrastructure</span></span>

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a><span data-ttu-id="96390-332">Förbereda infrastrukturen för arkitektur mall 1</span><span class="sxs-lookup"><span data-stu-id="96390-332">Prepare the infrastructure for Architectural Template 1</span></span>
<span data-ttu-id="96390-333">Azure Resource Manager-mallar för SAP underlätta distributionen av de nödvändiga resurserna.</span><span class="sxs-lookup"><span data-stu-id="96390-333">Azure Resource Manager templates for SAP help simplify deployment of required resources.</span></span>

<span data-ttu-id="96390-334">Trelagers-mallar i Azure Resource Manager stöder också scenarier med hög tillgänglighet som i arkitekturen mall 1, som har två kluster.</span><span class="sxs-lookup"><span data-stu-id="96390-334">The three-tier templates in Azure Resource Manager also support high-availability scenarios, such as in Architectural Template 1, which has two clusters.</span></span> <span data-ttu-id="96390-335">Varje kluster är ett SAP felpunkt för SAP ASCS/SCS och DBMS.</span><span class="sxs-lookup"><span data-stu-id="96390-335">Each cluster is an SAP single point of failure for SAP ASCS/SCS and DBMS.</span></span>

<span data-ttu-id="96390-336">Här är där du kan hämta Azure Resource Manager-mallar för exempelscenario som beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="96390-336">Here's where you can get Azure Resource Manager templates for the example scenario we describe in this article:</span></span>

* [<span data-ttu-id="96390-337">Azure Marketplace-avbildning</span><span class="sxs-lookup"><span data-stu-id="96390-337">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [<span data-ttu-id="96390-338">Anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="96390-338">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

<span data-ttu-id="96390-339">Förbereda infrastrukturen för arkitektur mall 1:</span><span class="sxs-lookup"><span data-stu-id="96390-339">To prepare the infrastructure for Architectural Template 1:</span></span>

- <span data-ttu-id="96390-340">I Azure-portalen på den **parametrar** blad i den **SYSTEMAVAILABILITY** väljer **HA**.</span><span class="sxs-lookup"><span data-stu-id="96390-340">In the Azure portal, on the **Parameters** blade, in the **SYSTEMAVAILABILITY** box, select **HA**.</span></span>

  ![Figur 11: Ange parametrar för SAP hög tillgänglighet Azure Resource Manager][sap-ha-guide-figure-3000]

<span data-ttu-id="96390-342">_**Figur 11:** ange parametrar för SAP hög tillgänglighet Azure Resource Manager_</span><span class="sxs-lookup"><span data-stu-id="96390-342">_**Figure 11:** Set SAP high-availability Azure Resource Manager parameters_</span></span>


  <span data-ttu-id="96390-343">Skapa mallar:</span><span class="sxs-lookup"><span data-stu-id="96390-343">The templates create:</span></span>

  * <span data-ttu-id="96390-344">**Virtuella datorer**:</span><span class="sxs-lookup"><span data-stu-id="96390-344">**Virtual machines**:</span></span>
    * <span data-ttu-id="96390-345">SAP programserver virtuella datorer: <*SAPSystemSID*> - di - <*tal*></span><span class="sxs-lookup"><span data-stu-id="96390-345">SAP Application Server virtual machines: <*SAPSystemSID*>-di-<*Number*></span></span>
    * <span data-ttu-id="96390-346">ASCS/SCS klustra virtuella datorer: <*SAPSystemSID*> - ascs - <*tal*></span><span class="sxs-lookup"><span data-stu-id="96390-346">ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-ascs-<*Number*></span></span>
    * <span data-ttu-id="96390-347">DBMS-kluster: <*SAPSystemSID*> - db - <*tal*></span><span class="sxs-lookup"><span data-stu-id="96390-347">DBMS cluster: <*SAPSystemSID*>-db-<*Number*></span></span>

  * <span data-ttu-id="96390-348">**Nätverkskort för alla virtuella datorer med tillhörande IP-adresser**:</span><span class="sxs-lookup"><span data-stu-id="96390-348">**Network cards for all virtual machines, with associated IP addresses**:</span></span>
    * <span data-ttu-id="96390-349"><*SAPSystemSID*> - nic - di - <*tal*></span><span class="sxs-lookup"><span data-stu-id="96390-349"><*SAPSystemSID*>-nic-di-<*Number*></span></span>
    * <span data-ttu-id="96390-350"><*SAPSystemSID*> - nic - ascs - <*tal*></span><span class="sxs-lookup"><span data-stu-id="96390-350"><*SAPSystemSID*>-nic-ascs-<*Number*></span></span>
    * <span data-ttu-id="96390-351"><*SAPSystemSID*> - nic - db - <*tal*></span><span class="sxs-lookup"><span data-stu-id="96390-351"><*SAPSystemSID*>-nic-db-<*Number*></span></span>

  * <span data-ttu-id="96390-352">**Azure storage-konton**</span><span class="sxs-lookup"><span data-stu-id="96390-352">**Azure storage accounts**</span></span>

  * <span data-ttu-id="96390-353">**Tillgänglighetsgrupper** för:</span><span class="sxs-lookup"><span data-stu-id="96390-353">**Availability groups** for:</span></span>
    * <span data-ttu-id="96390-354">SAP programserver virtuella datorer: <*SAPSystemSID*> - avset - di</span><span class="sxs-lookup"><span data-stu-id="96390-354">SAP Application Server virtual machines: <*SAPSystemSID*>-avset-di</span></span>
    * <span data-ttu-id="96390-355">SAP ASCS/SCS klustra virtuella datorer: <*SAPSystemSID*> - avset - ascs</span><span class="sxs-lookup"><span data-stu-id="96390-355">SAP ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-avset-ascs</span></span>
    * <span data-ttu-id="96390-356">DBMS klustra virtuella datorer: <*SAPSystemSID*> - avset - db</span><span class="sxs-lookup"><span data-stu-id="96390-356">DBMS cluster virtual machines: <*SAPSystemSID*>-avset-db</span></span>

  * <span data-ttu-id="96390-357">**Azure intern belastningsutjämnare**:</span><span class="sxs-lookup"><span data-stu-id="96390-357">**Azure internal load balancer**:</span></span>
    * <span data-ttu-id="96390-358">Med alla portar för ASCS/SCS-instansen och IP-adressen <*SAPSystemSID*> - lb - ascs</span><span class="sxs-lookup"><span data-stu-id="96390-358">With all ports for the ASCS/SCS instance and IP address <*SAPSystemSID*>-lb-ascs</span></span>
    * <span data-ttu-id="96390-359">Med alla portar för SQL Server DBMS och IP-adressen <*SAPSystemSID*> - lb - db</span><span class="sxs-lookup"><span data-stu-id="96390-359">With all ports for the SQL Server DBMS and IP address <*SAPSystemSID*>-lb-db</span></span>

  * <span data-ttu-id="96390-360">**Nätverkssäkerhetsgruppen**: <*SAPSystemSID*> - nsg - ascs-0</span><span class="sxs-lookup"><span data-stu-id="96390-360">**Network security group**: <*SAPSystemSID*>-nsg-ascs-0</span></span>  
    * <span data-ttu-id="96390-361">Med en öppen externa Remote Desktop Protocol (RDP)-port till den <*SAPSystemSID*> - ascs - 0 virtuell dator</span><span class="sxs-lookup"><span data-stu-id="96390-361">With an open external Remote Desktop Protocol (RDP) port to the <*SAPSystemSID*>-ascs-0 virtual machine</span></span>

> [!NOTE]
> <span data-ttu-id="96390-362">Alla IP-adresser för nätverkskort och Azure interna belastningsutjämnare **dynamisk** som standard.</span><span class="sxs-lookup"><span data-stu-id="96390-362">All IP addresses of the network cards and Azure internal load balancers are **dynamic** by default.</span></span> <span data-ttu-id="96390-363">Ändra dem till **Statiska** IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="96390-363">Change them to **static** IP addresses.</span></span> <span data-ttu-id="96390-364">Vi beskriver hur du gör det senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="96390-364">We describe how to do this later in the article.</span></span>
>
>

### <span data-ttu-id="96390-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Distribuera virtuella datorer med företagets nätverksanslutning (mellan platser) ska användas i produktion</span><span class="sxs-lookup"><span data-stu-id="96390-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Deploy virtual machines with corporate network connectivity (cross-premises) to use in production</span></span>
<span data-ttu-id="96390-366">Distribuera virtuella Azure-datorer med för produktion SAP-system, [företagets nätverksanslutning (mellan platser)] [ planning-guide-2.2] med hjälp av Azure plats-till-plats-VPN eller Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96390-366">For production SAP systems, deploy Azure virtual machines with [corporate network connectivity (cross-premises)][planning-guide-2.2] by using Azure Site-to-Site VPN or Azure ExpressRoute.</span></span>

> [!NOTE]
> <span data-ttu-id="96390-367">Du kan använda Azure Virtual Network-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-367">You can use your Azure Virtual Network instance.</span></span> <span data-ttu-id="96390-368">Virtuellt nätverk och undernät har redan skapats och förberetts.</span><span class="sxs-lookup"><span data-stu-id="96390-368">The virtual network and subnet have already been created and prepared.</span></span>
>
>

1.  <span data-ttu-id="96390-369">I Azure-portalen på den **parametrar** blad i den **NEWOREXISTINGSUBNET** väljer **befintliga**.</span><span class="sxs-lookup"><span data-stu-id="96390-369">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **existing**.</span></span>
2.  <span data-ttu-id="96390-370">I den **SUBNETID** Lägg till fullständig strängen förberedda Azure nätverkets SubnetID där du planerar att distribuera Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-370">In the **SUBNETID** box, add the full string of your prepared Azure network SubnetID where you plan to deploy your Azure virtual machines.</span></span>
3.  <span data-ttu-id="96390-371">Hämta en lista över alla undernät i Azure-nätverk, Kör PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="96390-371">To get a list of all Azure network subnets, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  <span data-ttu-id="96390-372">Den **ID** fältet visar den **SUBNETID**.</span><span class="sxs-lookup"><span data-stu-id="96390-372">The **ID** field shows the **SUBNETID**.</span></span>
4. <span data-ttu-id="96390-373">Att hämta en lista över alla **SUBNETID** värden, Kör PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="96390-373">To get a list of all **SUBNETID** values, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  <span data-ttu-id="96390-374">Den **SUBNETID** ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="96390-374">The **SUBNETID** looks like this:</span></span>

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <span data-ttu-id="96390-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Distribuera endast molnbaserad SAP-instanser för testning och demo</span><span class="sxs-lookup"><span data-stu-id="96390-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Deploy cloud-only SAP instances for test and demo</span></span>
<span data-ttu-id="96390-376">Du kan distribuera din SAP-systemet för hög tillgänglighet i en endast molnbaserad distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="96390-376">You can deploy your high-availability SAP system in a cloud-only deployment model.</span></span> <span data-ttu-id="96390-377">Den här typen av distribution är främst användbart för demonstration och testa användningsområden.</span><span class="sxs-lookup"><span data-stu-id="96390-377">This kind of deployment primarily is useful for demo and test use cases.</span></span> <span data-ttu-id="96390-378">Det är inte lämpligt för produktion användningsfall.</span><span class="sxs-lookup"><span data-stu-id="96390-378">It's not suited for production use cases.</span></span>

- <span data-ttu-id="96390-379">I Azure-portalen på den **parametrar** blad i den **NEWOREXISTINGSUBNET** väljer **nya**.</span><span class="sxs-lookup"><span data-stu-id="96390-379">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **new**.</span></span> <span data-ttu-id="96390-380">Lämna den **SUBNETID** fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="96390-380">Leave the **SUBNETID** field empty.</span></span>

  <span data-ttu-id="96390-381">SAP Azure Resource Manager-mallen skapar automatiskt den virtuella Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="96390-381">The SAP Azure Resource Manager template automatically creates the Azure virtual network and subnet.</span></span>

> [!NOTE]
> <span data-ttu-id="96390-382">Du måste distribuera minst en dedikerad virtuell dator för Active Directory och DNS i samma Azure Virtual Network-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-382">You also need to deploy at least one dedicated virtual machine for Active Directory and DNS in the same Azure Virtual Network instance.</span></span> <span data-ttu-id="96390-383">Mallen skapas inte de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="96390-383">The template doesn't create these virtual machines.</span></span>
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a><span data-ttu-id="96390-384">Förbereda infrastrukturen för arkitektur mall 2</span><span class="sxs-lookup"><span data-stu-id="96390-384">Prepare the infrastructure for Architectural Template 2</span></span>

<span data-ttu-id="96390-385">Du kan använda den här Azure Resource Manager-mallen för SAP för att underlätta distributionen av infrastrukturresurser för SAP arkitektur mall 2.</span><span class="sxs-lookup"><span data-stu-id="96390-385">You can use this Azure Resource Manager template for SAP to help simplify deployment of required infrastructure resources for SAP Architectural Template 2.</span></span>

<span data-ttu-id="96390-386">Här är där du kan hämta Azure Resource Manager-mallar för det här distributionsscenariot:</span><span class="sxs-lookup"><span data-stu-id="96390-386">Here's where you can get Azure Resource Manager templates for this deployment scenario:</span></span>

* [<span data-ttu-id="96390-387">Azure Marketplace-avbildning</span><span class="sxs-lookup"><span data-stu-id="96390-387">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [<span data-ttu-id="96390-388">Anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="96390-388">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a><span data-ttu-id="96390-389">Förbereda infrastrukturen för arkitektur mall 3</span><span class="sxs-lookup"><span data-stu-id="96390-389">Prepare the infrastructure for Architectural Template 3</span></span>

<span data-ttu-id="96390-390">Du kan förbereda infrastrukturen och konfigurera SAP för **multi-SID**.</span><span class="sxs-lookup"><span data-stu-id="96390-390">You can prepare the infrastructure and configure SAP for **multi-SID**.</span></span> <span data-ttu-id="96390-391">Du kan exempelvis lägga till en ytterligare SAP ASCS/SCS-instans i en *befintliga* klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="96390-391">For example, you can add an additional SAP ASCS/SCS instance into an *existing* cluster configuration.</span></span> <span data-ttu-id="96390-392">Mer information finns i [konfigurera en ytterligare SAP ASCS/SCS-instans i en befintlig klusterkonfiguration för att skapa en konfiguration för SAP multi-SID i Azure Resource Manager][sap-ha-multi-sid-guide].</span><span class="sxs-lookup"><span data-stu-id="96390-392">For more information, see [Configure an additional SAP ASCS/SCS instance into an existing cluster configuration to create an SAP multi-SID configuration in Azure Resource Manager][sap-ha-multi-sid-guide].</span></span>

<span data-ttu-id="96390-393">Om du vill skapa ett nytt multi-SID-kluster, kan du använda multi-SID [snabbstartsmallar på GitHub](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="96390-393">If you want to create a new multi-SID cluster, you can use the multi-SID [quickstart templates on GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span>
<span data-ttu-id="96390-394">Om du vill skapa ett nytt multi-SID-kluster måste du distribuera följande tre mallar:</span><span class="sxs-lookup"><span data-stu-id="96390-394">To create a new multi-SID cluster, you need to deploy the following three templates:</span></span>

* [<span data-ttu-id="96390-395">ASCS/SCS mall</span><span class="sxs-lookup"><span data-stu-id="96390-395">ASCS/SCS template</span></span>](#ASCS-SCS-template)
* [<span data-ttu-id="96390-396">Databasmall för</span><span class="sxs-lookup"><span data-stu-id="96390-396">Database template</span></span>](#database-template)
* [<span data-ttu-id="96390-397">Mall för servrar</span><span class="sxs-lookup"><span data-stu-id="96390-397">Application servers template</span></span>](#application-servers-template)

<span data-ttu-id="96390-398">I följande avsnitt har mer information om mallar och parametrar måste du ange i mallar.</span><span class="sxs-lookup"><span data-stu-id="96390-398">The following sections have more details about the templates and the parameters you need to provide in the templates.</span></span>

#### <span data-ttu-id="96390-399"><a name="ASCS-SCS-template"></a>ASCS/SCS mall</span><span class="sxs-lookup"><span data-stu-id="96390-399"><a name="ASCS-SCS-template"></a> ASCS/SCS template</span></span>

<span data-ttu-id="96390-400">ASCS/SCS mallen distribuerar två virtuella datorer som du kan använda för att skapa ett redundanskluster i Windows Server som är värd för flera ASCS/SCS-instanser.</span><span class="sxs-lookup"><span data-stu-id="96390-400">The ASCS/SCS template deploys two virtual machines that you can use to create a Windows Server failover cluster that hosts multiple ASCS/SCS instances.</span></span>

<span data-ttu-id="96390-401">Att konfigurera mallen ASCS/SCS multi-SID i den [ASCS/SCS multi-SID-mall][sap-templates-3-tier-multisid-xscs-marketplace-image], ange värden för följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="96390-401">To set up the ASCS/SCS multi-SID template, in the [ASCS/SCS multi-SID template][sap-templates-3-tier-multisid-xscs-marketplace-image], enter values for the following parameters:</span></span>

  - <span data-ttu-id="96390-402">**Resursen prefixet**.</span><span class="sxs-lookup"><span data-stu-id="96390-402">**Resource Prefix**.</span></span>  <span data-ttu-id="96390-403">Ange resurs-prefixet, vilket är visas för alla resurser som har skapats under distributionen.</span><span class="sxs-lookup"><span data-stu-id="96390-403">Set the resource prefix, which is used to prefix all resources that are created during the deployment.</span></span> <span data-ttu-id="96390-404">Resurserna som inte hör till endast en SAP-system, är prefixet för resursen inte eftersom SID för en SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-404">Because the resources do not belong to only one SAP system, the prefix of the resource is not the SID of one SAP system.</span></span>  <span data-ttu-id="96390-405">Prefixet måste vara mellan **tre till sex tecken**.</span><span class="sxs-lookup"><span data-stu-id="96390-405">The prefix must be between **three and six characters**.</span></span>
  - <span data-ttu-id="96390-406">**Stacken typen**.</span><span class="sxs-lookup"><span data-stu-id="96390-406">**Stack Type**.</span></span> <span data-ttu-id="96390-407">Välj stack-typ av SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-407">Select the stack type of the SAP system.</span></span> <span data-ttu-id="96390-408">Beroende på stack-typ har Azure belastningsutjämnare en (ABAP eller endast Java) eller två (ABAP + Java) privata IP-adresser per SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-408">Depending on the stack type, Azure Load Balancer has one (ABAP or Java only) or two (ABAP+Java) private IP addresses per SAP system.</span></span>
  -  <span data-ttu-id="96390-409">**OS-typen**.</span><span class="sxs-lookup"><span data-stu-id="96390-409">**OS Type**.</span></span> <span data-ttu-id="96390-410">Välj operativsystem för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="96390-410">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="96390-411">**SAP System antal**.</span><span class="sxs-lookup"><span data-stu-id="96390-411">**SAP System Count**.</span></span> <span data-ttu-id="96390-412">Välj antal SAP-system som du vill installera i det här klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-412">Select the number of SAP systems you want to install in this cluster.</span></span>
  -  <span data-ttu-id="96390-413">**Filsystemets tillgänglighet**.</span><span class="sxs-lookup"><span data-stu-id="96390-413">**System Availability**.</span></span> <span data-ttu-id="96390-414">Välj **HA**.</span><span class="sxs-lookup"><span data-stu-id="96390-414">Select **HA**.</span></span>
  -  <span data-ttu-id="96390-415">**Användarnamn och lösenord för Admin administratör**.</span><span class="sxs-lookup"><span data-stu-id="96390-415">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="96390-416">Skapa en ny användare som kan användas för att logga in på datorn.</span><span class="sxs-lookup"><span data-stu-id="96390-416">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="96390-417">**Ny eller befintlig undernät**.</span><span class="sxs-lookup"><span data-stu-id="96390-417">**New Or Existing Subnet**.</span></span> <span data-ttu-id="96390-418">Ange om ett nytt virtuellt nätverk och undernät som ska skapas eller ett befintligt undernät som ska användas.</span><span class="sxs-lookup"><span data-stu-id="96390-418">Set whether a new virtual network and subnet should be created, or an existing subnet should be used.</span></span> <span data-ttu-id="96390-419">Om du redan har ett virtuellt nätverk som är anslutna till ditt lokala nätverk väljer **befintliga**.</span><span class="sxs-lookup"><span data-stu-id="96390-419">If you already have a virtual network that is connected to your on-premises network, select **existing**.</span></span>
  -  <span data-ttu-id="96390-420">**Undernät-Id**. Ange ID för det undernät som de virtuella datorerna ska anslutas.</span><span class="sxs-lookup"><span data-stu-id="96390-420">**Subnet Id**. Set the ID of the subnet to which the virtual machines should be connected.</span></span> <span data-ttu-id="96390-421">Välj undernät virtuellt privat nätverk (VPN) eller ExpressRoute virtuellt nätverk att ansluta den virtuella datorn till ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="96390-421">Select the subnet of your virtual private network (VPN) or ExpressRoute virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="96390-422">ID: T ser oftast ut så här:</span><span class="sxs-lookup"><span data-stu-id="96390-422">The ID usually looks like this:</span></span>

   <span data-ttu-id="96390-423">/subscriptions/ <*prenumerations-id*> /resourceGroups/ <*resursgruppnamn*> /providers/Microsoft.Network/virtualNetworks/ <*virtuella nätverksnamnet*> /subnets/ <*undernätsnamn*></span><span class="sxs-lookup"><span data-stu-id="96390-423">/subscriptions/<*subscription id*>/resourceGroups/<*resource group name*>/providers/Microsoft.Network/virtualNetworks/<*virtual network name*>/subnets/<*subnet name*></span></span>

<span data-ttu-id="96390-424">Mallen distribuerar en belastningsutjämnare för Azure-instans som har stöd för flera SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-424">The template deploys one Azure Load Balancer instance, which supports multiple SAP systems.</span></span>

- <span data-ttu-id="96390-425">ASCS-instanserna är konfigurerade för instansnummer 00, 10, 20...</span><span class="sxs-lookup"><span data-stu-id="96390-425">The ASCS instances are configured for instance number 00, 10, 20...</span></span>
- <span data-ttu-id="96390-426">SCS-instanserna är konfigurerade för instansnummer 01, 11, 21...</span><span class="sxs-lookup"><span data-stu-id="96390-426">The SCS instances are configured for instance number 01, 11, 21...</span></span>
- <span data-ttu-id="96390-427">ASCS sätta replikering Server (ERS) (endast Linux)-instanserna är konfigurerade för instansnummer 02, 12, 22...</span><span class="sxs-lookup"><span data-stu-id="96390-427">The ASCS Enqueue Replication Server (ERS) (Linux only) instances are configured for instance number 02, 12, 22...</span></span>
- <span data-ttu-id="96390-428">SCS ERS (endast Linux)-instanserna är konfigurerade för instansnummer 03, 13, 23...</span><span class="sxs-lookup"><span data-stu-id="96390-428">The SCS ERS (Linux only) instances are configured for instance number 03, 13, 23...</span></span>

<span data-ttu-id="96390-429">Belastningsutjämnaren innehåller 1.2 för Linux VIP(s), 1 x-VIP för ASCS/SCS och 1 x-VIP för ÄNDARE (endast Linux).</span><span class="sxs-lookup"><span data-stu-id="96390-429">The load balancer contains 1 (2 for Linux) VIP(s), 1x VIP for ASCS/SCS and 1x VIP for ERS (Linux only).</span></span>

<span data-ttu-id="96390-430">Följande lista innehåller alla belastningsutjämningsregler (där x är numret på SAP-systemet, till exempel 1, 2, 3...):</span><span class="sxs-lookup"><span data-stu-id="96390-430">The following list contains all load balancing rules (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="96390-431">Windows-specifika portar för varje system för SAP: 445, 5985</span><span class="sxs-lookup"><span data-stu-id="96390-431">Windows-specific ports for every SAP system: 445, 5985</span></span>
- <span data-ttu-id="96390-432">ASCS-portar (instansnummer x0): 32 x 0, 36 x 0, 39 x 0, 81 0 x, 5 x 013, 5 x 014, 5 x 016</span><span class="sxs-lookup"><span data-stu-id="96390-432">ASCS ports (instance number x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016</span></span>
- <span data-ttu-id="96390-433">SCS portar (instansnummer x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116</span><span class="sxs-lookup"><span data-stu-id="96390-433">SCS ports (instance number x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116</span></span>
- <span data-ttu-id="96390-434">ASCS ERS portar på Linux (instansnummer x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216</span><span class="sxs-lookup"><span data-stu-id="96390-434">ASCS ERS ports on Linux (instance number x2): 33x2, 5x213, 5x214, 5x216</span></span>
- <span data-ttu-id="96390-435">SCS ERS portar på Linux (instansnummer x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316</span><span class="sxs-lookup"><span data-stu-id="96390-435">SCS ERS ports on Linux (instance number x3): 33x3, 5x313, 5x314, 5x316</span></span>

<span data-ttu-id="96390-436">Belastningsutjämnaren har konfigurerats för att använda följande avsökningen portar (där x är numret på SAP-systemet, till exempel 1, 2, 3...):</span><span class="sxs-lookup"><span data-stu-id="96390-436">The load balancer is configured to use the following probe ports (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="96390-437">ASCS/SCS interna läsa in belastningsutjämning avsökningsport: 620 x 0</span><span class="sxs-lookup"><span data-stu-id="96390-437">ASCS/SCS internal load balancer probe port: 620x0</span></span>
- <span data-ttu-id="96390-438">ÄNDARE interna läsa in belastningsutjämning avsökningsport (endast Linux): 621 x 2</span><span class="sxs-lookup"><span data-stu-id="96390-438">ERS internal load balancer probe port (Linux only): 621x2</span></span>

#### <span data-ttu-id="96390-439"><a name="database-template"></a>Databasmall för</span><span class="sxs-lookup"><span data-stu-id="96390-439"><a name="database-template"></a> Database template</span></span>

<span data-ttu-id="96390-440">Mallen databasen distribuerar en eller två virtuella datorer som du kan använda för att installera systemet för relationsdatabashantering (RDBMS) för en SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-440">The database template deploys one or two virtual machines that you can use to install the relational database management system (RDBMS) for one SAP system.</span></span> <span data-ttu-id="96390-441">Om du distribuerar en ASCS/SCS-mall för fem SAP-system, måste du distribuera den här mallen fem gånger.</span><span class="sxs-lookup"><span data-stu-id="96390-441">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="96390-442">Att konfigurera mallen databasen multi-SID i den [multi-SID-mall för databas][sap-templates-3-tier-multisid-db-marketplace-image], ange värden för följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="96390-442">To set up the database multi-SID template, in the [database multi-SID template][sap-templates-3-tier-multisid-db-marketplace-image], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="96390-443">**SAP System-Id**. Ange ID för SAP-system för SAP-system som du vill installera.</span><span class="sxs-lookup"><span data-stu-id="96390-443">**Sap System Id**. Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="96390-444">ID som ska användas som ett prefix för de resurser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="96390-444">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="96390-445">**OS-typen**.</span><span class="sxs-lookup"><span data-stu-id="96390-445">**Os Type**.</span></span> <span data-ttu-id="96390-446">Välj operativsystem för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="96390-446">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="96390-447">**DbType**.</span><span class="sxs-lookup"><span data-stu-id="96390-447">**Dbtype**.</span></span> <span data-ttu-id="96390-448">Välj typ av databas som du vill installera i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-448">Select the type of the database you want to install on the cluster.</span></span> <span data-ttu-id="96390-449">Välj **SQL** om du vill installera Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="96390-449">Select **SQL** if you want to install Microsoft SQL Server.</span></span> <span data-ttu-id="96390-450">Välj **HANA** om du planerar att installera SAP HANA på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="96390-450">Select **HANA** if you plan to install SAP HANA on the virtual machines.</span></span> <span data-ttu-id="96390-451">Se till att välja rätt operativsystemtypen: Välj **Windows** för SQL och välj en Linux-distributionsplats för HANA.</span><span class="sxs-lookup"><span data-stu-id="96390-451">Make sure to select the correct operating system type: select **Windows** for SQL, and select a Linux distribution for HANA.</span></span> <span data-ttu-id="96390-452">Azure-belastningsutjämnaren som är anslutna till virtuella datorer kommer att konfigureras för att stödja den valda databastyp:</span><span class="sxs-lookup"><span data-stu-id="96390-452">The Azure Load Balancer that is connected to the virtual machines will be configured to support the selected database type:</span></span>
    * <span data-ttu-id="96390-453">**SQL**.</span><span class="sxs-lookup"><span data-stu-id="96390-453">**SQL**.</span></span> <span data-ttu-id="96390-454">Belastningsutjämnaren att belastningsutjämna port 1433.</span><span class="sxs-lookup"><span data-stu-id="96390-454">The load balancer will load-balance port 1433.</span></span> <span data-ttu-id="96390-455">Se till att använda den här porten för din SQL Server Always On-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="96390-455">Make sure to use this port for your SQL Server Always On setup.</span></span>
    * <span data-ttu-id="96390-456">**HANA**.</span><span class="sxs-lookup"><span data-stu-id="96390-456">**HANA**.</span></span> <span data-ttu-id="96390-457">Belastningsutjämnaren att belastningsutjämna portarna 35015 och 35017.</span><span class="sxs-lookup"><span data-stu-id="96390-457">The load balancer will load-balance ports 35015 and 35017.</span></span> <span data-ttu-id="96390-458">Se till att installera SAP HANA med instansnummer **50**.</span><span class="sxs-lookup"><span data-stu-id="96390-458">Make sure to install SAP HANA with instance number **50**.</span></span>
    <span data-ttu-id="96390-459">Belastningsutjämnaren använder avsökningsport 62550.</span><span class="sxs-lookup"><span data-stu-id="96390-459">The load balancer will use probe port 62550.</span></span>
  -  <span data-ttu-id="96390-460">**SAP systemstorlek**.</span><span class="sxs-lookup"><span data-stu-id="96390-460">**Sap System Size**.</span></span> <span data-ttu-id="96390-461">Ange antalet SAP ger det nya systemet.</span><span class="sxs-lookup"><span data-stu-id="96390-461">Set the number of SAPS the new system will provide.</span></span> <span data-ttu-id="96390-462">Om du inte vet hur många SAP systemet kräver, be din SAP-teknikpartner eller systemintegreraren.</span><span class="sxs-lookup"><span data-stu-id="96390-462">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="96390-463">**Filsystemets tillgänglighet**.</span><span class="sxs-lookup"><span data-stu-id="96390-463">**System Availability**.</span></span> <span data-ttu-id="96390-464">Välj **HA**.</span><span class="sxs-lookup"><span data-stu-id="96390-464">Select **HA**.</span></span>
  -  <span data-ttu-id="96390-465">**Användarnamn och lösenord för Admin administratör**.</span><span class="sxs-lookup"><span data-stu-id="96390-465">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="96390-466">Skapa en ny användare som kan användas för att logga in på datorn.</span><span class="sxs-lookup"><span data-stu-id="96390-466">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="96390-467">**Undernät-Id**. Ange ID för det undernät som du använde under distributionen av mallen ASCS/SCS eller ID för det undernät som har skapats som en del av malldistribution ASCS/SCS.</span><span class="sxs-lookup"><span data-stu-id="96390-467">**Subnet Id**. Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>

#### <span data-ttu-id="96390-468"><a name="application-servers-template"></a>Mall för servrar</span><span class="sxs-lookup"><span data-stu-id="96390-468"><a name="application-servers-template"></a> Application servers template</span></span>

<span data-ttu-id="96390-469">Servrar programmall distribuerar två eller flera virtuella datorer som kan användas som SAP Application Server-instanser för en SAP-system.</span><span class="sxs-lookup"><span data-stu-id="96390-469">The application servers template deploys two or more virtual machines that can be used as SAP Application Server instances for one SAP system.</span></span> <span data-ttu-id="96390-470">Om du distribuerar en ASCS/SCS-mall för fem SAP-system, måste du distribuera den här mallen fem gånger.</span><span class="sxs-lookup"><span data-stu-id="96390-470">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="96390-471">Att ställa in programmet servrar multi-SID-mall i den [programmall servrar multi-SID][sap-templates-3-tier-multisid-apps-marketplace-image], ange värden för följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="96390-471">To set up the application servers multi-SID template, in the [application servers multi-SID template][sap-templates-3-tier-multisid-apps-marketplace-image], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="96390-472">**SAP System-Id**. Ange ID för SAP-system för SAP-system som du vill installera.</span><span class="sxs-lookup"><span data-stu-id="96390-472">**Sap System Id**. Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="96390-473">ID som ska användas som ett prefix för de resurser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="96390-473">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="96390-474">**OS-typen**.</span><span class="sxs-lookup"><span data-stu-id="96390-474">**Os Type**.</span></span> <span data-ttu-id="96390-475">Välj operativsystem för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="96390-475">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="96390-476">**SAP systemstorlek**.</span><span class="sxs-lookup"><span data-stu-id="96390-476">**Sap System Size**.</span></span> <span data-ttu-id="96390-477">Antal SAP ger det nya systemet.</span><span class="sxs-lookup"><span data-stu-id="96390-477">The number of SAPS the new system will provide.</span></span> <span data-ttu-id="96390-478">Om du inte vet hur många SAP systemet kräver, be din SAP-teknikpartner eller systemintegreraren.</span><span class="sxs-lookup"><span data-stu-id="96390-478">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="96390-479">**Filsystemets tillgänglighet**.</span><span class="sxs-lookup"><span data-stu-id="96390-479">**System Availability**.</span></span> <span data-ttu-id="96390-480">Välj **HA**.</span><span class="sxs-lookup"><span data-stu-id="96390-480">Select **HA**.</span></span>
  -  <span data-ttu-id="96390-481">**Användarnamn och lösenord för Admin administratör**.</span><span class="sxs-lookup"><span data-stu-id="96390-481">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="96390-482">Skapa en ny användare som kan användas för att logga in på datorn.</span><span class="sxs-lookup"><span data-stu-id="96390-482">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="96390-483">**Undernät-Id**. Ange ID för det undernät som du använde under distributionen av mallen ASCS/SCS eller ID för det undernät som har skapats som en del av malldistribution ASCS/SCS.</span><span class="sxs-lookup"><span data-stu-id="96390-483">**Subnet Id**. Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>


### <span data-ttu-id="96390-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuella Azure-nätverket</span><span class="sxs-lookup"><span data-stu-id="96390-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure virtual network</span></span>
<span data-ttu-id="96390-485">I vårt exempel är i virtuella Azure-nätverket adressutrymme 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="96390-485">In our example, the address space of the Azure virtual network is 10.0.0.0/16.</span></span> <span data-ttu-id="96390-486">Det finns ett undernät som kallas **undernät**, med en-adressintervallet 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="96390-486">There is one subnet called **Subnet**, with an address range of 10.0.0.0/24.</span></span> <span data-ttu-id="96390-487">Alla virtuella datorer och interna belastningsutjämnare har distribuerats i det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="96390-487">All virtual machines and internal load balancers are deployed in this virtual network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96390-488">Inte göra ändringar i nätverksinställningar i gästoperativsystemet.</span><span class="sxs-lookup"><span data-stu-id="96390-488">Don't make any changes to the network settings inside the guest operating system.</span></span> <span data-ttu-id="96390-489">Detta inkluderar IP-adresser, DNS-servrar och undernät.</span><span class="sxs-lookup"><span data-stu-id="96390-489">This includes IP addresses, DNS servers, and subnet.</span></span> <span data-ttu-id="96390-490">Konfigurera nätverksinställningarna i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-490">Configure all your network settings in Azure.</span></span> <span data-ttu-id="96390-491">Tjänsten DHCP Dynamic Host Configuration Protocol () sprider dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="96390-491">The Dynamic Host Configuration Protocol (DHCP) service propagates your settings.</span></span>
>
>

### <span data-ttu-id="96390-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP-adresser</span><span class="sxs-lookup"><span data-stu-id="96390-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP addresses</span></span>

<span data-ttu-id="96390-493">Om du vill ange krävs DNS IP-adresser, gör du följande steg.</span><span class="sxs-lookup"><span data-stu-id="96390-493">To set the required DNS IP addresses, do the following steps.</span></span>

1.  <span data-ttu-id="96390-494">I Azure-portalen på den **DNS-servrar** bladet, se till att det virtuella nätverket **DNS-servrar** alternativet är inställt på **anpassad DNS**.</span><span class="sxs-lookup"><span data-stu-id="96390-494">In the Azure portal, on the **DNS servers** blade, make sure that your virtual network **DNS servers** option is set to **Custom DNS**.</span></span>
2.  <span data-ttu-id="96390-495">Välj dina inställningar beroende på vilken typ av nätverk som du har.</span><span class="sxs-lookup"><span data-stu-id="96390-495">Select your settings based on the type of network you have.</span></span> <span data-ttu-id="96390-496">Mer information finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="96390-496">For more information, see the following resources:</span></span>
    * <span data-ttu-id="96390-497">[Företagets nätverksanslutning (mellan platser)][planning-guide-2.2]: Lägg till IP-adresserna för de lokala DNS-servrarna.</span><span class="sxs-lookup"><span data-stu-id="96390-497">[Corporate network connectivity (cross-premises)][planning-guide-2.2]: Add the IP addresses of the on-premises DNS servers.</span></span>  
    <span data-ttu-id="96390-498">Du kan utöka lokala DNS-servrar till de virtuella datorerna som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-498">You can extend on-premises DNS servers to the virtual machines that are running in Azure.</span></span> <span data-ttu-id="96390-499">Du kan lägga till IP-adresserna för de virtuella Azure-datorer som du kör DNS-tjänsten i det scenariot.</span><span class="sxs-lookup"><span data-stu-id="96390-499">In that scenario, you can add the IP addresses of the Azure virtual machines on which you run the DNS service.</span></span>
    * <span data-ttu-id="96390-500">[Endast molnbaserad distribution][planning-guide-2.1]: distribuera en ytterligare virtuell dator i samma virtuella nätverk-instans som fungerar som en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="96390-500">[Cloud-only deployment][planning-guide-2.1]: Deploy an additional virtual machine in the same Virtual Network instance that serves as a DNS server.</span></span> <span data-ttu-id="96390-501">Lägg till IP-adresserna för de virtuella Azure-datorer som du har konfigurerat för att köra DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="96390-501">Add the IP addresses of the Azure virtual machines that you've set up to run DNS service.</span></span>

    ![Figur 12: Konfigurera DNS-servrar för Azure-nätverk][sap-ha-guide-figure-3001]

    <span data-ttu-id="96390-503">_**Figur 12:** konfigurera DNS-servrar för Azure-nätverk_</span><span class="sxs-lookup"><span data-stu-id="96390-503">_**Figure 12:** Configure DNS servers for Azure Virtual Network_</span></span>

  > [!NOTE]
  > <span data-ttu-id="96390-504">Du måste starta om de virtuella Azure-datorerna för att tillämpa ändringen och sprida de nya DNS-servrarna om du ändrar IP-adresser för DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="96390-504">If you change the IP addresses of the DNS servers, you need to restart the Azure virtual machines to apply the change and propagate the new DNS servers.</span></span>
  >
  >

<span data-ttu-id="96390-505">I vårt exempel har DNS-tjänsten installerats och konfigurerats på dessa virtuella Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="96390-505">In our example, the DNS service is installed and configured on these Windows virtual machines:</span></span>

| <span data-ttu-id="96390-506">Rollen virtuell dator</span><span class="sxs-lookup"><span data-stu-id="96390-506">Virtual machine role</span></span> | <span data-ttu-id="96390-507">Värdnamn för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="96390-507">Virtual machine host name</span></span> | <span data-ttu-id="96390-508">Namn på nätverkskort</span><span class="sxs-lookup"><span data-stu-id="96390-508">Network card name</span></span> | <span data-ttu-id="96390-509">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="96390-509">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="96390-510">Första DNS-servern</span><span class="sxs-lookup"><span data-stu-id="96390-510">First DNS server</span></span> |<span data-ttu-id="96390-511">domcontr 0</span><span class="sxs-lookup"><span data-stu-id="96390-511">domcontr-0</span></span> |<span data-ttu-id="96390-512">PR1-nic-domcontr-0</span><span class="sxs-lookup"><span data-stu-id="96390-512">pr1-nic-domcontr-0</span></span> |<span data-ttu-id="96390-513">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="96390-513">10.0.0.10</span></span> |
| <span data-ttu-id="96390-514">Andra DNS-server</span><span class="sxs-lookup"><span data-stu-id="96390-514">Second DNS server</span></span> |<span data-ttu-id="96390-515">domcontr 1</span><span class="sxs-lookup"><span data-stu-id="96390-515">domcontr-1</span></span> |<span data-ttu-id="96390-516">PR1-nic-domcontr-1</span><span class="sxs-lookup"><span data-stu-id="96390-516">pr1-nic-domcontr-1</span></span> |<span data-ttu-id="96390-517">10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="96390-517">10.0.0.11</span></span> |

### <span data-ttu-id="96390-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Värdnamn och statiska IP-adresser för SAP ASCS/SCS klustrade instansen och DBMS klustrad instans</span><span class="sxs-lookup"><span data-stu-id="96390-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Host names and static IP addresses for the SAP ASCS/SCS clustered instance and DBMS clustered instance</span></span>

<span data-ttu-id="96390-519">För lokal distribution måste dessa reserverade värdnamn och IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="96390-519">For on-premises deployment, you need these reserved host names and IP addresses:</span></span>

| <span data-ttu-id="96390-520">Virtuell värd namn roll</span><span class="sxs-lookup"><span data-stu-id="96390-520">Virtual host name role</span></span> | <span data-ttu-id="96390-521">Virtuella värdnamn</span><span class="sxs-lookup"><span data-stu-id="96390-521">Virtual host name</span></span> | <span data-ttu-id="96390-522">Virtuell statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="96390-522">Virtual static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="96390-523">SAP ASCS/SCS första klustret virtuellt värdnamn (för hantering av kluster)</span><span class="sxs-lookup"><span data-stu-id="96390-523">SAP ASCS/SCS first cluster virtual host name (for cluster management)</span></span> |<span data-ttu-id="96390-524">PR1-ascs-vir</span><span class="sxs-lookup"><span data-stu-id="96390-524">pr1-ascs-vir</span></span> |<span data-ttu-id="96390-525">10.0.0.42</span><span class="sxs-lookup"><span data-stu-id="96390-525">10.0.0.42</span></span> |
| <span data-ttu-id="96390-526">SAP ASCS/SCS virtuell värd instansnamn</span><span class="sxs-lookup"><span data-stu-id="96390-526">SAP ASCS/SCS instance virtual host name</span></span> |<span data-ttu-id="96390-527">PR1-ascs-sap</span><span class="sxs-lookup"><span data-stu-id="96390-527">pr1-ascs-sap</span></span> |<span data-ttu-id="96390-528">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="96390-528">10.0.0.43</span></span> |
| <span data-ttu-id="96390-529">SAP DBMS andra kluster virtuellt värdnamn (hantering)</span><span class="sxs-lookup"><span data-stu-id="96390-529">SAP DBMS second cluster virtual host name (cluster management)</span></span> |<span data-ttu-id="96390-530">PR1-dbms-vir</span><span class="sxs-lookup"><span data-stu-id="96390-530">pr1-dbms-vir</span></span> |<span data-ttu-id="96390-531">10.0.0.32</span><span class="sxs-lookup"><span data-stu-id="96390-531">10.0.0.32</span></span> |

<span data-ttu-id="96390-532">När du skapar klustret kan du skapa virtuella värdnamn **pr1-ascs-vir** och **pr1-dbms-vir** och associerade IP-adresser som hantera själva klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-532">When you create the cluster, create the virtual host names **pr1-ascs-vir** and **pr1-dbms-vir** and the associated IP addresses that manage the cluster itself.</span></span> <span data-ttu-id="96390-533">Information om hur du gör detta finns [samla in klusternoder i en klusterkonfiguration][sap-ha-guide-8.12.1].</span><span class="sxs-lookup"><span data-stu-id="96390-533">For information about how to do this, see [Collect cluster nodes in a cluster configuration][sap-ha-guide-8.12.1].</span></span>

<span data-ttu-id="96390-534">Du kan manuellt skapa de andra två virtuella värdnamn, **pr1-ascs-sap** och **pr1-dbms-sap**, och de associerade IP-adresserna på DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="96390-534">You can manually create the other two virtual host names, **pr1-ascs-sap** and **pr1-dbms-sap**, and the associated IP addresses, on the DNS server.</span></span> <span data-ttu-id="96390-535">Använd dessa resurser den klustrade instansen SAP ASCS/SCS och den klustrade DBMS-instansen.</span><span class="sxs-lookup"><span data-stu-id="96390-535">The clustered SAP ASCS/SCS instance and the clustered DBMS instance use these resources.</span></span> <span data-ttu-id="96390-536">Information om hur du gör detta finns [skapa ett virtuellt värdnamn för en klustrad instans av SAP ASCS/SCS][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="96390-536">For information about how to do this, see [Create a virtual host name for a clustered SAP ASCS/SCS instance][sap-ha-guide-9.1.1].</span></span>

### <span data-ttu-id="96390-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Ange statiska IP-adresser för de virtuella datorerna för SAP</span><span class="sxs-lookup"><span data-stu-id="96390-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Set static IP addresses for the SAP virtual machines</span></span>
<span data-ttu-id="96390-538">När du distribuerar virtuella datorer i klustret måste du ange statiska IP-adresser för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-538">After you deploy the virtual machines to use in your cluster, you need to set static IP addresses for all virtual machines.</span></span> <span data-ttu-id="96390-539">Gör detta i Azure Virtual Network-konfigurationen och inte i gästoperativsystemet.</span><span class="sxs-lookup"><span data-stu-id="96390-539">Do this in the Azure Virtual Network configuration, and not in the guest operating system.</span></span>

1.  <span data-ttu-id="96390-540">Välj i Azure-portalen **resursgruppen** > **nätverkskort** > **inställningar** > **IP-adress**.</span><span class="sxs-lookup"><span data-stu-id="96390-540">In the Azure portal, select **Resource Group** > **Network Card** > **Settings** > **IP Address**.</span></span>
2.  <span data-ttu-id="96390-541">På den **IP-adresser** bladet under **tilldelning**väljer **statiska**.</span><span class="sxs-lookup"><span data-stu-id="96390-541">On the **IP addresses** blade, under **Assignment**, select **Static**.</span></span> <span data-ttu-id="96390-542">I den **IP-adress** ange IP-adressen som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="96390-542">In the **IP address** box, enter the IP address that you want to use.</span></span>

  > [!NOTE]
  > <span data-ttu-id="96390-543">Om du ändrar IP-adressen för nätverkskortet, måste du starta om de virtuella Azure-datorerna för att tillämpa ändringen.</span><span class="sxs-lookup"><span data-stu-id="96390-543">If you change the IP address of the network card, you need to restart the Azure virtual machines to apply the change.</span></span>  
  >
  >

  ![Figur 13: Ange statiska IP-adresser för nätverkskort på varje virtuell dator][sap-ha-guide-figure-3002]

  <span data-ttu-id="96390-545">_**Figur 13:** ange statiska IP-adresser för nätverkskort på varje virtuell dator_</span><span class="sxs-lookup"><span data-stu-id="96390-545">_**Figure 13:** Set static IP addresses for the network card of each virtual machine_</span></span>

  <span data-ttu-id="96390-546">Upprepa det här steget för alla nätverksgränssnitt som är, för alla virtuella datorer, inklusive virtuella datorer som du vill använda för din Active Directory/DNS-tjänst.</span><span class="sxs-lookup"><span data-stu-id="96390-546">Repeat this step for all network interfaces, that is, for all virtual machines, including virtual machines that you want to use for your Active Directory/DNS service.</span></span>

<span data-ttu-id="96390-547">I vårt exempel har vi dessa virtuella datorer och statiska IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="96390-547">In our example, we have these virtual machines and static IP addresses:</span></span>

| <span data-ttu-id="96390-548">Rollen virtuell dator</span><span class="sxs-lookup"><span data-stu-id="96390-548">Virtual machine role</span></span> | <span data-ttu-id="96390-549">Värdnamn för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="96390-549">Virtual machine host name</span></span> | <span data-ttu-id="96390-550">Namn på nätverkskort</span><span class="sxs-lookup"><span data-stu-id="96390-550">Network card name</span></span> | <span data-ttu-id="96390-551">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="96390-551">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="96390-552">Första SAP Application Server-instansen</span><span class="sxs-lookup"><span data-stu-id="96390-552">First SAP Application Server instance</span></span> |<span data-ttu-id="96390-553">PR1-di-0</span><span class="sxs-lookup"><span data-stu-id="96390-553">pr1-di-0</span></span> |<span data-ttu-id="96390-554">PR1-nic-di-0</span><span class="sxs-lookup"><span data-stu-id="96390-554">pr1-nic-di-0</span></span> |<span data-ttu-id="96390-555">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="96390-555">10.0.0.50</span></span> |
| <span data-ttu-id="96390-556">Andra SAP Application Server-instans</span><span class="sxs-lookup"><span data-stu-id="96390-556">Second SAP Application Server instance</span></span> |<span data-ttu-id="96390-557">PR1-di-1</span><span class="sxs-lookup"><span data-stu-id="96390-557">pr1-di-1</span></span> |<span data-ttu-id="96390-558">PR1-nic-di-1</span><span class="sxs-lookup"><span data-stu-id="96390-558">pr1-nic-di-1</span></span> |<span data-ttu-id="96390-559">10.0.0.51</span><span class="sxs-lookup"><span data-stu-id="96390-559">10.0.0.51</span></span> |
| <span data-ttu-id="96390-560">...</span><span class="sxs-lookup"><span data-stu-id="96390-560">...</span></span> |<span data-ttu-id="96390-561">...</span><span class="sxs-lookup"><span data-stu-id="96390-561">...</span></span> |<span data-ttu-id="96390-562">...</span><span class="sxs-lookup"><span data-stu-id="96390-562">...</span></span> |<span data-ttu-id="96390-563">...</span><span class="sxs-lookup"><span data-stu-id="96390-563">...</span></span> |
| <span data-ttu-id="96390-564">Senaste SAP Application Server-instans</span><span class="sxs-lookup"><span data-stu-id="96390-564">Last SAP Application Server instance</span></span> |<span data-ttu-id="96390-565">PR1-di-5</span><span class="sxs-lookup"><span data-stu-id="96390-565">pr1-di-5</span></span> |<span data-ttu-id="96390-566">PR1-nic-di-5</span><span class="sxs-lookup"><span data-stu-id="96390-566">pr1-nic-di-5</span></span> |<span data-ttu-id="96390-567">10.0.0.55</span><span class="sxs-lookup"><span data-stu-id="96390-567">10.0.0.55</span></span> |
| <span data-ttu-id="96390-568">Första klusternoden för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-568">First cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="96390-569">PR1-ascs-0</span><span class="sxs-lookup"><span data-stu-id="96390-569">pr1-ascs-0</span></span> |<span data-ttu-id="96390-570">PR1-nic-ascs-0</span><span class="sxs-lookup"><span data-stu-id="96390-570">pr1-nic-ascs-0</span></span> |<span data-ttu-id="96390-571">10.0.0.40</span><span class="sxs-lookup"><span data-stu-id="96390-571">10.0.0.40</span></span> |
| <span data-ttu-id="96390-572">Andra klusternod för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-572">Second cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="96390-573">PR1-ascs-1</span><span class="sxs-lookup"><span data-stu-id="96390-573">pr1-ascs-1</span></span> |<span data-ttu-id="96390-574">PR1-nic-ascs-1</span><span class="sxs-lookup"><span data-stu-id="96390-574">pr1-nic-ascs-1</span></span> |<span data-ttu-id="96390-575">10.0.0.41</span><span class="sxs-lookup"><span data-stu-id="96390-575">10.0.0.41</span></span> |
| <span data-ttu-id="96390-576">Första klusternoden för DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-576">First cluster node for DBMS instance</span></span> |<span data-ttu-id="96390-577">PR1-db-0</span><span class="sxs-lookup"><span data-stu-id="96390-577">pr1-db-0</span></span> |<span data-ttu-id="96390-578">PR1-nic-db-0</span><span class="sxs-lookup"><span data-stu-id="96390-578">pr1-nic-db-0</span></span> |<span data-ttu-id="96390-579">10.0.0.30</span><span class="sxs-lookup"><span data-stu-id="96390-579">10.0.0.30</span></span> |
| <span data-ttu-id="96390-580">Andra klusternod för DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-580">Second cluster node for DBMS instance</span></span> |<span data-ttu-id="96390-581">PR1-db-1</span><span class="sxs-lookup"><span data-stu-id="96390-581">pr1-db-1</span></span> |<span data-ttu-id="96390-582">PR1-nic-db-1</span><span class="sxs-lookup"><span data-stu-id="96390-582">pr1-nic-db-1</span></span> |<span data-ttu-id="96390-583">10.0.0.31</span><span class="sxs-lookup"><span data-stu-id="96390-583">10.0.0.31</span></span> |

### <span data-ttu-id="96390-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Ange en statisk IP-adress för den interna belastningsutjämnaren som Azure</span><span class="sxs-lookup"><span data-stu-id="96390-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Set a static IP address for the Azure internal load balancer</span></span>

<span data-ttu-id="96390-585">SAP Azure Resource Manager-mallen skapar en Azure intern belastningsutjämnare som används för det SAP ASCS/SCS instans och DBMS-klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-585">The SAP Azure Resource Manager template creates an Azure internal load balancer that is used for the SAP ASCS/SCS instance cluster and the DBMS cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96390-586">IP-adressen för den virtuella värdnamnet för SAP ASCS/SCS är samma som IP-adressen för den interna belastningsutjämnaren SAP ASCS/SCS: **pr1-lb-ascs**.</span><span class="sxs-lookup"><span data-stu-id="96390-586">The IP address of the virtual host name of the SAP ASCS/SCS is the same as the IP address of the SAP ASCS/SCS internal load balancer: **pr1-lb-ascs**.</span></span>
> <span data-ttu-id="96390-587">IP-adressen för det virtuella namnet på DBMS är samma som IP-adressen för den interna belastningsutjämnaren DBMS: **pr1-lb-dbms**.</span><span class="sxs-lookup"><span data-stu-id="96390-587">The IP address of the virtual name of the DBMS is the same as the IP address of the DBMS internal load balancer: **pr1-lb-dbms**.</span></span>
>
>

<span data-ttu-id="96390-588">Ange en statisk IP-adress för den interna belastningsutjämnaren som Azure:</span><span class="sxs-lookup"><span data-stu-id="96390-588">To set a static IP address for the Azure internal load balancer:</span></span>

1.  <span data-ttu-id="96390-589">Den första distributionen anger interna IP-adressen för belastningsutjämnaren till **dynamiska**.</span><span class="sxs-lookup"><span data-stu-id="96390-589">The initial deployment sets the internal load balancer IP address to **Dynamic**.</span></span> <span data-ttu-id="96390-590">I Azure-portalen på den **IP-adresser** bladet under **tilldelning**väljer **statiska**.</span><span class="sxs-lookup"><span data-stu-id="96390-590">In the Azure portal, on the **IP addresses** blade, under **Assignment**, select **Static**.</span></span>
2.  <span data-ttu-id="96390-591">Ange IP-adressen för den interna belastningsutjämnaren **pr1-lb-ascs** till IP-adressen för den virtuella värdnamnet för SAP ASCS/SCS-instansen.</span><span class="sxs-lookup"><span data-stu-id="96390-591">Set the IP address of the internal load balancer **pr1-lb-ascs** to the IP address of the virtual host name of the SAP ASCS/SCS instance.</span></span>
3.  <span data-ttu-id="96390-592">Ange IP-adressen för den interna belastningsutjämnaren **pr1-lb-dbms** IP-adressen för den virtuella värdnamnet för DBMS-instansen.</span><span class="sxs-lookup"><span data-stu-id="96390-592">Set the IP address of the internal load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

  ![Figur 14: Ange statiska IP-adresser för den interna belastningsutjämnaren för SAP ASCS/SCS-instans][sap-ha-guide-figure-3003]

  <span data-ttu-id="96390-594">_**Figur 14:** ange statiska IP-adresser för den interna belastningsutjämnaren för SAP ASCS/SCS-instans_</span><span class="sxs-lookup"><span data-stu-id="96390-594">_**Figure 14:** Set static IP addresses for the internal load balancer for the SAP ASCS/SCS instance_</span></span>

<span data-ttu-id="96390-595">I vårt exempel har vi två Azure interna belastningsutjämnare som har dessa statiska IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="96390-595">In our example, we have two Azure internal load balancers that have these static IP addresses:</span></span>

| <span data-ttu-id="96390-596">Azure interna belastningsutjämnarrollen</span><span class="sxs-lookup"><span data-stu-id="96390-596">Azure internal load balancer role</span></span> | <span data-ttu-id="96390-597">Azure interna belastningsutjämnarens namn</span><span class="sxs-lookup"><span data-stu-id="96390-597">Azure internal load balancer name</span></span> | <span data-ttu-id="96390-598">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="96390-598">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="96390-599">Interna belastningsutjämnare för SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-599">SAP ASCS/SCS instance internal load balancer</span></span> |<span data-ttu-id="96390-600">ascs-lb-PR1</span><span class="sxs-lookup"><span data-stu-id="96390-600">pr1-lb-ascs</span></span> |<span data-ttu-id="96390-601">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="96390-601">10.0.0.43</span></span> |
| <span data-ttu-id="96390-602">SAP DBMS intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="96390-602">SAP DBMS internal load balancer</span></span> |<span data-ttu-id="96390-603">PR1-lb-dbms</span><span class="sxs-lookup"><span data-stu-id="96390-603">pr1-lb-dbms</span></span> |<span data-ttu-id="96390-604">10.0.0.33</span><span class="sxs-lookup"><span data-stu-id="96390-604">10.0.0.33</span></span> |


### <span data-ttu-id="96390-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Standard ASCS/SCS belastningsutjämningsregler för Azure interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="96390-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Default ASCS/SCS load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="96390-606">SAP Azure Resource Manager-mallen skapar de portar som du behöver:</span><span class="sxs-lookup"><span data-stu-id="96390-606">The SAP Azure Resource Manager template creates the ports you need:</span></span>
* <span data-ttu-id="96390-607">En ABAP ASCS-instans med standard-instansnummer **00**</span><span class="sxs-lookup"><span data-stu-id="96390-607">An ABAP ASCS instance, with the default instance number **00**</span></span>
* <span data-ttu-id="96390-608">En Java-SCS-instans med standard-instansnummer **01**</span><span class="sxs-lookup"><span data-stu-id="96390-608">A Java SCS instance, with the default instance number **01**</span></span>

<span data-ttu-id="96390-609">När du installerar din SAP ASCS/SCS-instans måste du använda instansen Standardnumret **00** för ABAP ASCS-instansen och instansen Standardnumret **01** för din Java SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-609">When you install your SAP ASCS/SCS instance, you must use the default instance number **00** for your ABAP ASCS instance and the default instance number **01** for your Java SCS instance.</span></span>

<span data-ttu-id="96390-610">Skapa sedan nödvändiga intern belastningsutjämning slutpunkter för SAP NetWeaver-portar.</span><span class="sxs-lookup"><span data-stu-id="96390-610">Next, create required internal load balancing endpoints for the SAP NetWeaver ports.</span></span>

<span data-ttu-id="96390-611">Om du vill skapa nödvändiga intern belastningsutjämning slutpunkter först skapa dessa slutpunkter för SAP NetWeaver ABAP ASCS-portar för belastningsutjämning:</span><span class="sxs-lookup"><span data-stu-id="96390-611">To create required internal load balancing endpoints, first, create these load balancing endpoints for the SAP NetWeaver ABAP ASCS ports:</span></span>

| <span data-ttu-id="96390-612">Tjänsten/belastningen belastningsutjämning Regelnamn</span><span class="sxs-lookup"><span data-stu-id="96390-612">Service/load balancing rule name</span></span> | <span data-ttu-id="96390-613">Standardportnumren.</span><span class="sxs-lookup"><span data-stu-id="96390-613">Default port numbers</span></span> | <span data-ttu-id="96390-614">Konkret portar för (ASCS-instans med instansnummer 00) (ERS med 10)</span><span class="sxs-lookup"><span data-stu-id="96390-614">Concrete ports for (ASCS instance with instance number 00) (ERS with 10)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="96390-615">Sätta Server / *lbrule3200*</span><span class="sxs-lookup"><span data-stu-id="96390-615">Enqueue Server / *lbrule3200*</span></span> |<span data-ttu-id="96390-616">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-616">32<*InstanceNumber*></span></span> |<span data-ttu-id="96390-617">3200</span><span class="sxs-lookup"><span data-stu-id="96390-617">3200</span></span> |
| <span data-ttu-id="96390-618">ABAP Message Server / *lbrule3600*</span><span class="sxs-lookup"><span data-stu-id="96390-618">ABAP Message Server / *lbrule3600*</span></span> |<span data-ttu-id="96390-619">36 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-619">36<*InstanceNumber*></span></span> |<span data-ttu-id="96390-620">3600</span><span class="sxs-lookup"><span data-stu-id="96390-620">3600</span></span> |
| <span data-ttu-id="96390-621">Internt ABAP meddelande / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="96390-621">Internal ABAP Message / *lbrule3900*</span></span> |<span data-ttu-id="96390-622">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-622">39<*InstanceNumber*></span></span> |<span data-ttu-id="96390-623">3900</span><span class="sxs-lookup"><span data-stu-id="96390-623">3900</span></span> |
| <span data-ttu-id="96390-624">Serverns HTTP-meddelandet / *Lbrule8100*</span><span class="sxs-lookup"><span data-stu-id="96390-624">Message Server HTTP / *Lbrule8100*</span></span> |<span data-ttu-id="96390-625">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-625">81<*InstanceNumber*></span></span> |<span data-ttu-id="96390-626">8100</span><span class="sxs-lookup"><span data-stu-id="96390-626">8100</span></span> |
| <span data-ttu-id="96390-627">SAP Start Service ASCS HTTP / *Lbrule50013*</span><span class="sxs-lookup"><span data-stu-id="96390-627">SAP Start Service ASCS HTTP / *Lbrule50013*</span></span> |<span data-ttu-id="96390-628">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="96390-628">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="96390-629">50013</span><span class="sxs-lookup"><span data-stu-id="96390-629">50013</span></span> |
| <span data-ttu-id="96390-630">SAP Start Service ASCS HTTPS / *Lbrule50014*</span><span class="sxs-lookup"><span data-stu-id="96390-630">SAP Start Service ASCS HTTPS / *Lbrule50014*</span></span> |<span data-ttu-id="96390-631">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="96390-631">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="96390-632">50014</span><span class="sxs-lookup"><span data-stu-id="96390-632">50014</span></span> |
| <span data-ttu-id="96390-633">Sätta replikering / *Lbrule50016*</span><span class="sxs-lookup"><span data-stu-id="96390-633">Enqueue Replication / *Lbrule50016*</span></span> |<span data-ttu-id="96390-634">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="96390-634">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="96390-635">50016</span><span class="sxs-lookup"><span data-stu-id="96390-635">50016</span></span> |
| <span data-ttu-id="96390-636">SAP Start Service ÄNDARE HTTP *Lbrule51013*</span><span class="sxs-lookup"><span data-stu-id="96390-636">SAP Start Service ERS HTTP *Lbrule51013*</span></span> |<span data-ttu-id="96390-637">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="96390-637">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="96390-638">51013</span><span class="sxs-lookup"><span data-stu-id="96390-638">51013</span></span> |
| <span data-ttu-id="96390-639">SAP Start Service ÄNDARE HTTP *Lbrule51014*</span><span class="sxs-lookup"><span data-stu-id="96390-639">SAP Start Service ERS HTTP *Lbrule51014*</span></span> |<span data-ttu-id="96390-640">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="96390-640">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="96390-641">51014</span><span class="sxs-lookup"><span data-stu-id="96390-641">51014</span></span> |
| <span data-ttu-id="96390-642">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="96390-642">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="96390-643">5985</span><span class="sxs-lookup"><span data-stu-id="96390-643">5985</span></span> |
| <span data-ttu-id="96390-644">Filresurs *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="96390-644">File Share *Lbrule445*</span></span> | |<span data-ttu-id="96390-645">445</span><span class="sxs-lookup"><span data-stu-id="96390-645">445</span></span> |

<span data-ttu-id="96390-646">_**Tabell 1:** portnummer för SAP NetWeaver ABAP ASCS-instanser_</span><span class="sxs-lookup"><span data-stu-id="96390-646">_**Table 1:** Port numbers of the SAP NetWeaver ABAP ASCS instances_</span></span>

<span data-ttu-id="96390-647">Skapa sedan dessa slutpunkter för SAP NetWeaver Java SCS-portar för belastningsutjämning:</span><span class="sxs-lookup"><span data-stu-id="96390-647">Then, create these load balancing endpoints for the SAP NetWeaver Java SCS ports:</span></span>

| <span data-ttu-id="96390-648">Tjänsten/belastningen belastningsutjämning Regelnamn</span><span class="sxs-lookup"><span data-stu-id="96390-648">Service/load balancing rule name</span></span> | <span data-ttu-id="96390-649">Standardportnumren.</span><span class="sxs-lookup"><span data-stu-id="96390-649">Default port numbers</span></span> | <span data-ttu-id="96390-650">Konkret portar för (SCS-instans med instansnummer 01) (ERS med 11)</span><span class="sxs-lookup"><span data-stu-id="96390-650">Concrete ports for (SCS instance with instance number 01) (ERS with 11)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="96390-651">Sätta Server / *lbrule3201*</span><span class="sxs-lookup"><span data-stu-id="96390-651">Enqueue Server / *lbrule3201*</span></span> |<span data-ttu-id="96390-652">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-652">32<*InstanceNumber*></span></span> |<span data-ttu-id="96390-653">3201</span><span class="sxs-lookup"><span data-stu-id="96390-653">3201</span></span> |
| <span data-ttu-id="96390-654">Gateway-Server / *lbrule3301*</span><span class="sxs-lookup"><span data-stu-id="96390-654">Gateway Server / *lbrule3301*</span></span> |<span data-ttu-id="96390-655">33 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-655">33<*InstanceNumber*></span></span> |<span data-ttu-id="96390-656">3301</span><span class="sxs-lookup"><span data-stu-id="96390-656">3301</span></span> |
| <span data-ttu-id="96390-657">Java Message Server / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="96390-657">Java Message Server / *lbrule3900*</span></span> |<span data-ttu-id="96390-658">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-658">39<*InstanceNumber*></span></span> |<span data-ttu-id="96390-659">3901</span><span class="sxs-lookup"><span data-stu-id="96390-659">3901</span></span> |
| <span data-ttu-id="96390-660">Serverns HTTP-meddelandet / *Lbrule8101*</span><span class="sxs-lookup"><span data-stu-id="96390-660">Message Server HTTP / *Lbrule8101*</span></span> |<span data-ttu-id="96390-661">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="96390-661">81<*InstanceNumber*></span></span> |<span data-ttu-id="96390-662">8101</span><span class="sxs-lookup"><span data-stu-id="96390-662">8101</span></span> |
| <span data-ttu-id="96390-663">SAP Start Service SCS HTTP / *Lbrule50113*</span><span class="sxs-lookup"><span data-stu-id="96390-663">SAP Start Service SCS HTTP / *Lbrule50113*</span></span> |<span data-ttu-id="96390-664">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="96390-664">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="96390-665">50113</span><span class="sxs-lookup"><span data-stu-id="96390-665">50113</span></span> |
| <span data-ttu-id="96390-666">SAP Start Service SCS HTTPS / *Lbrule50114*</span><span class="sxs-lookup"><span data-stu-id="96390-666">SAP Start Service SCS HTTPS / *Lbrule50114*</span></span> |<span data-ttu-id="96390-667">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="96390-667">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="96390-668">50114</span><span class="sxs-lookup"><span data-stu-id="96390-668">50114</span></span> |
| <span data-ttu-id="96390-669">Sätta replikering / *Lbrule50116*</span><span class="sxs-lookup"><span data-stu-id="96390-669">Enqueue Replication / *Lbrule50116*</span></span> |<span data-ttu-id="96390-670">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="96390-670">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="96390-671">50116</span><span class="sxs-lookup"><span data-stu-id="96390-671">50116</span></span> |
| <span data-ttu-id="96390-672">SAP Start Service ÄNDARE HTTP *Lbrule51113*</span><span class="sxs-lookup"><span data-stu-id="96390-672">SAP Start Service ERS HTTP *Lbrule51113*</span></span> |<span data-ttu-id="96390-673">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="96390-673">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="96390-674">51113</span><span class="sxs-lookup"><span data-stu-id="96390-674">51113</span></span> |
| <span data-ttu-id="96390-675">SAP Start Service ÄNDARE HTTP *Lbrule51114*</span><span class="sxs-lookup"><span data-stu-id="96390-675">SAP Start Service ERS HTTP *Lbrule51114*</span></span> |<span data-ttu-id="96390-676">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="96390-676">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="96390-677">51114</span><span class="sxs-lookup"><span data-stu-id="96390-677">51114</span></span> |
| <span data-ttu-id="96390-678">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="96390-678">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="96390-679">5985</span><span class="sxs-lookup"><span data-stu-id="96390-679">5985</span></span> |
| <span data-ttu-id="96390-680">Filresurs *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="96390-680">File Share *Lbrule445*</span></span> | |<span data-ttu-id="96390-681">445</span><span class="sxs-lookup"><span data-stu-id="96390-681">445</span></span> |

<span data-ttu-id="96390-682">_**Tabell 2:** portnummer för SAP NetWeaver Java SCS-instanser_</span><span class="sxs-lookup"><span data-stu-id="96390-682">_**Table 2:** Port numbers of the SAP NetWeaver Java SCS instances_</span></span>

![Figur 15: Standard ASCS/SCS belastningsutjämningsregler för Azure interna belastningsutjämnare][sap-ha-guide-figure-3004]

<span data-ttu-id="96390-684">_**Figur 15:** standard ASCS/SCS regler för Azure intern belastningsutjämnare för belastningsutjämning_</span><span class="sxs-lookup"><span data-stu-id="96390-684">_**Figure 15:** Default ASCS/SCS load balancing rules for the Azure internal load balancer_</span></span>

<span data-ttu-id="96390-685">Ange IP-adressen för belastningsutjämnaren **pr1-lb-dbms** IP-adressen för den virtuella värdnamnet för DBMS-instansen.</span><span class="sxs-lookup"><span data-stu-id="96390-685">Set the IP address of the load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

### <span data-ttu-id="96390-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Ändra ASCS/SCS standard belastningsutjämning regler för Azure interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="96390-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Change the ASCS/SCS default load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="96390-687">Om du vill använda andra värden för SAP ASCS eller SCS instanser måste du ändra namn och värden på deras hamnar från standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="96390-687">If you want to use different numbers for the SAP ASCS or SCS instances, you must change the names and values of their ports from default values.</span></span>

1.  <span data-ttu-id="96390-688">Välj i Azure-portalen  **<* SID*> - lb - ascs läsa in belastningsutjämning ** > **regler för att läsa in belastningsutjämning**.</span><span class="sxs-lookup"><span data-stu-id="96390-688">In the Azure portal, select **<*SID*>-lb-ascs load balancer** > **Load Balancing Rules**.</span></span>
2.  <span data-ttu-id="96390-689">För alla belastningsutjämningsregler som tillhör instansen SAP ASCS eller SCS, ändra dessa värden:</span><span class="sxs-lookup"><span data-stu-id="96390-689">For all load balancing rules that belong to the SAP ASCS or SCS instance, change these values:</span></span>

  * <span data-ttu-id="96390-690">Namn</span><span class="sxs-lookup"><span data-stu-id="96390-690">Name</span></span>
  * <span data-ttu-id="96390-691">Port</span><span class="sxs-lookup"><span data-stu-id="96390-691">Port</span></span>
  * <span data-ttu-id="96390-692">Backend-port</span><span class="sxs-lookup"><span data-stu-id="96390-692">Back-end port</span></span>

  <span data-ttu-id="96390-693">Om du vill ändra standardvärdet för ASCS-instans från 00 och 31, måste du göra ändringar för alla portar som anges i tabell 1.</span><span class="sxs-lookup"><span data-stu-id="96390-693">For example, if you want to change the default ASCS instance number from 00 to 31, you need to make the changes for all ports listed in Table 1.</span></span>

  <span data-ttu-id="96390-694">Här är ett exempel på en uppdatering för port *lbrule3200*.</span><span class="sxs-lookup"><span data-stu-id="96390-694">Here's an example of an update for port *lbrule3200*.</span></span>

  ![Bild 16: Ändra ASCS/SCS standard belastningsutjämning regler för Azure interna belastningsutjämnare][sap-ha-guide-figure-3005]

  <span data-ttu-id="96390-696">_**Bild 16:** ändra ASCS/SCS standard belastningsutjämning regler för Azure interna belastningsutjämnare_</span><span class="sxs-lookup"><span data-stu-id="96390-696">_**Figure 16:** Change the ASCS/SCS default load balancing rules for the Azure internal load balancer_</span></span>

### <span data-ttu-id="96390-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Lägga till Windows-datorer i domänen</span><span class="sxs-lookup"><span data-stu-id="96390-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Add Windows virtual machines to the domain</span></span>

<span data-ttu-id="96390-698">När du tilldelar en statisk IP-adress till de virtuella datorerna kan du lägga till de virtuella datorerna till domänen.</span><span class="sxs-lookup"><span data-stu-id="96390-698">After you assign a static IP address to the virtual machines, add the virtual machines to the domain.</span></span>

![Figur 17: Lägga till en virtuell dator i en domän][sap-ha-guide-figure-3006]

<span data-ttu-id="96390-700">_**Figur 17:** lägga till en virtuell dator i en domän_</span><span class="sxs-lookup"><span data-stu-id="96390-700">_**Figure 17:** Add a virtual machine to a domain_</span></span>

### <span data-ttu-id="96390-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Lägg till registerposter på båda klusternoderna för SAP ASCS/SCS-instansen</span><span class="sxs-lookup"><span data-stu-id="96390-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Add registry entries on both cluster nodes of the SAP ASCS/SCS instance</span></span>

<span data-ttu-id="96390-702">Azure belastningsutjämnare har en intern belastningsutjämnare att stängs anslutningar när anslutningar är inaktiva för en viss tid (en timeout för inaktivitet).</span><span class="sxs-lookup"><span data-stu-id="96390-702">Azure Load Balancer has an internal load balancer that closes connections when the connections are idle for a set period of time (an idle timeout).</span></span> <span data-ttu-id="96390-703">SAP arbetsprocesser i dialogrutan instanser öppna anslutningar till SAP sätta bearbeta så snart som de första sätta/status Created begäran måste skickas.</span><span class="sxs-lookup"><span data-stu-id="96390-703">SAP work processes in dialog instances open connections to the SAP enqueue process as soon as the first enqueue/dequeue request needs to be sent.</span></span> <span data-ttu-id="96390-704">Dessa anslutningar förblir oftast etablerade förrän arbetsprocessen eller sätta processen startas om.</span><span class="sxs-lookup"><span data-stu-id="96390-704">These connections usually remain established until the work process or the enqueue process restarts.</span></span> <span data-ttu-id="96390-705">Om anslutningen är inaktiv under en angiven tidsperiod, stängs den interna belastningsutjämnaren som Azure anslutningar.</span><span class="sxs-lookup"><span data-stu-id="96390-705">However, if the connection is idle for a set period of time, the Azure internal load balancer closes the connections.</span></span> <span data-ttu-id="96390-706">Detta är inte ett problem Eftersom arbetsprocessen för SAP återupprättar anslutningen till sätta processen om det inte längre finns.</span><span class="sxs-lookup"><span data-stu-id="96390-706">This isn't a problem because the SAP work process reestablishes the connection to the enqueue process if it no longer exists.</span></span> <span data-ttu-id="96390-707">Dessa aktiviteter finns dokumenterade i developer spår av SAP processer, men de skapar en stor mängd extra innehåll i dessa spår.</span><span class="sxs-lookup"><span data-stu-id="96390-707">These activities are documented in the developer traces of SAP processes, but they create a large amount of extra content in those traces.</span></span> <span data-ttu-id="96390-708">Det är en bra idé att ändra TCP/IP `KeepAliveTime` och `KeepAliveInterval` på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="96390-708">It's a good idea to change the TCP/IP `KeepAliveTime` and `KeepAliveInterval` on both cluster nodes.</span></span> <span data-ttu-id="96390-709">Kombinera ändringarna i TCP/IP-parametrar med SAP profil-parametrar som beskrivs senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="96390-709">Combine these changes in the TCP/IP parameters with SAP profile parameters, described later in the article.</span></span>

<span data-ttu-id="96390-710">För att lägga till poster i registret på båda klusternoderna för SAP ASCS/SCS-instansen, Lägg först till dessa registerposter i Windows på båda klusternoderna i Windows för SAP ASCS/SCS:</span><span class="sxs-lookup"><span data-stu-id="96390-710">To add registry entries on both cluster nodes of the SAP ASCS/SCS instance, first, add these Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="96390-711">Sökväg</span><span class="sxs-lookup"><span data-stu-id="96390-711">Path</span></span> | <span data-ttu-id="96390-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="96390-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="96390-713">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="96390-713">Variable name</span></span> |`KeepAliveTime` |
| <span data-ttu-id="96390-714">Variabeltyp</span><span class="sxs-lookup"><span data-stu-id="96390-714">Variable type</span></span> |<span data-ttu-id="96390-715">REG_DWORD (decimalt)</span><span class="sxs-lookup"><span data-stu-id="96390-715">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="96390-716">Värde</span><span class="sxs-lookup"><span data-stu-id="96390-716">Value</span></span> |<span data-ttu-id="96390-717">120000</span><span class="sxs-lookup"><span data-stu-id="96390-717">120000</span></span> |
| <span data-ttu-id="96390-718">Länka till dokumentationen</span><span class="sxs-lookup"><span data-stu-id="96390-718">Link to documentation</span></span> |[<span data-ttu-id="96390-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span><span class="sxs-lookup"><span data-stu-id="96390-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

<span data-ttu-id="96390-720">_**Tabell 3:** ändra den första parametern för TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="96390-720">_**Table 3:** Change the first TCP/IP parameter_</span></span>

<span data-ttu-id="96390-721">Lägg sedan till den här Windows-registerposter på båda klusternoderna i Windows för SAP ASCS/SCS:</span><span class="sxs-lookup"><span data-stu-id="96390-721">Then, add this Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="96390-722">Sökväg</span><span class="sxs-lookup"><span data-stu-id="96390-722">Path</span></span> | <span data-ttu-id="96390-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="96390-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="96390-724">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="96390-724">Variable name</span></span> |`KeepAliveInterval` |
| <span data-ttu-id="96390-725">Variabeltyp</span><span class="sxs-lookup"><span data-stu-id="96390-725">Variable type</span></span> |<span data-ttu-id="96390-726">REG_DWORD (decimalt)</span><span class="sxs-lookup"><span data-stu-id="96390-726">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="96390-727">Värde</span><span class="sxs-lookup"><span data-stu-id="96390-727">Value</span></span> |<span data-ttu-id="96390-728">120000</span><span class="sxs-lookup"><span data-stu-id="96390-728">120000</span></span> |
| <span data-ttu-id="96390-729">Länka till dokumentationen</span><span class="sxs-lookup"><span data-stu-id="96390-729">Link to documentation</span></span> |[<span data-ttu-id="96390-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span><span class="sxs-lookup"><span data-stu-id="96390-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

<span data-ttu-id="96390-731">_**Tabell 4:** ändra andra TCP/IP-parameter_</span><span class="sxs-lookup"><span data-stu-id="96390-731">_**Table 4:** Change the second TCP/IP parameter_</span></span>

<span data-ttu-id="96390-732">**Om du vill tillämpa ändringarna genom att starta om båda klusternoderna**.</span><span class="sxs-lookup"><span data-stu-id="96390-732">**To apply the changes, restart both cluster nodes**.</span></span>

### <span data-ttu-id="96390-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Konfigurera ett redundanskluster i Windows Server-kluster för en SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Set up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance</span></span>

<span data-ttu-id="96390-734">Konfigurera ett redundanskluster i Windows Server-kluster för en SAP ASCS/SCS-instans innebär att dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="96390-734">Setting up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="96390-735">Insamling av klusternoderna i en klusterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="96390-735">Collecting the cluster nodes in a cluster configuration</span></span>
- <span data-ttu-id="96390-736">Konfigurera ett filresursvittne för kluster</span><span class="sxs-lookup"><span data-stu-id="96390-736">Configuring a cluster file share witness</span></span>

#### <span data-ttu-id="96390-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Samla in klusternoderna i en klusterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="96390-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Collect the cluster nodes in a cluster configuration</span></span>

1.  <span data-ttu-id="96390-738">Guiden Lägg till roller och funktioner, lägga till redundansklustring till båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="96390-738">In the Add Role and Features Wizard, add failover clustering to both cluster nodes.</span></span>
2.  <span data-ttu-id="96390-739">Ställ in failover-kluster med hjälp av hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="96390-739">Set up the failover cluster by using Failover Cluster Manager.</span></span> <span data-ttu-id="96390-740">I hanteraren för redundanskluster, Välj **Skapa kluster**, och Lägg sedan till bara namnet på det första klustret nod A. Lägg inte till den andra noden ännu; Du måste lägga till den andra noden i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="96390-740">In Failover Cluster Manager, select **Create Cluster**, and then add only the name of the first cluster, node A. Do not add the second node yet; you'll add the second node in a later step.</span></span>

  ![Figur 18: Lägg till namnet på servern eller den virtuella datorn på den första noden i klustret][sap-ha-guide-figure-3007]

  <span data-ttu-id="96390-742">_**Figur 18:** Lägg till namnet på servern eller den virtuella datorn på den första noden i klustret_</span><span class="sxs-lookup"><span data-stu-id="96390-742">_**Figure 18:** Add the server or virtual machine name of the first cluster node_</span></span>

3.  <span data-ttu-id="96390-743">Ange nätverksnamn (virtuellt värdnamn) i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-743">Enter the network name (virtual host name) of the cluster.</span></span>

  ![Bild 19: Ange klusternamnet][sap-ha-guide-figure-3008]

  <span data-ttu-id="96390-745">_**Bild 19:** ange klusternamnet_</span><span class="sxs-lookup"><span data-stu-id="96390-745">_**Figure 19:** Enter the cluster name_</span></span>

4.  <span data-ttu-id="96390-746">När du har skapat klustret, kör du ett klustervalideringstest.</span><span class="sxs-lookup"><span data-stu-id="96390-746">After you've created the cluster, run a cluster validation test.</span></span>

  ![Figur 20: Kör verifieringen kluster][sap-ha-guide-figure-3009]

  <span data-ttu-id="96390-748">_**Figur 20:** kör verifieringen kluster_</span><span class="sxs-lookup"><span data-stu-id="96390-748">_**Figure 20:** Run the cluster validation check_</span></span>

  <span data-ttu-id="96390-749">Du kan ignorera alla varningar om diskarna nu i processen.</span><span class="sxs-lookup"><span data-stu-id="96390-749">You can ignore any warnings about disks at this point in the process.</span></span> <span data-ttu-id="96390-750">Du ska lägga till ett filresursvittne och SIOS delade diskar senare.</span><span class="sxs-lookup"><span data-stu-id="96390-750">You'll add a file share witness and the SIOS shared disks later.</span></span> <span data-ttu-id="96390-751">I det här skedet behöver du inte bekymra dig om att ha ett kvorum.</span><span class="sxs-lookup"><span data-stu-id="96390-751">At this stage, you don't need to worry about having a quorum.</span></span>

  ![Figur 21: Inga kvorumdisken hittas][sap-ha-guide-figure-3010]

  <span data-ttu-id="96390-753">_**Figur 21:** inga kvorumdisken hittas_</span><span class="sxs-lookup"><span data-stu-id="96390-753">_**Figure 21:** No quorum disk is found_</span></span>

  ![Figur 22: Core klusterresurs måste en ny IP-adress][sap-ha-guide-figure-3011]

  <span data-ttu-id="96390-755">_**Figur 22:** Core klusterresurs måste en ny IP-adress_</span><span class="sxs-lookup"><span data-stu-id="96390-755">_**Figure 22:** Core cluster resource needs a new IP address_</span></span>

5.  <span data-ttu-id="96390-756">Ändra IP-adressen för klustertjänsten kärnor.</span><span class="sxs-lookup"><span data-stu-id="96390-756">Change the IP address of the core cluster service.</span></span> <span data-ttu-id="96390-757">Klustret kan inte starta tills du ändrar IP-adressen för klustertjänsten core eftersom IP-adressen för servern som pekar på en av noderna virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="96390-757">The cluster can't start until you change the IP address of the core cluster service, because the IP address of the server points to one of the virtual machine nodes.</span></span> <span data-ttu-id="96390-758">Gör detta på den **egenskaper** sidan core klustertjänsten IP-resurs.</span><span class="sxs-lookup"><span data-stu-id="96390-758">Do this on the **Properties** page of the core cluster service's IP resource.</span></span>

  <span data-ttu-id="96390-759">Till exempel vi måste du tilldela en IP-adress (i vårt exempel **10.0.0.42**) för det virtuella värdnamnet för klustret **pr1-ascs-vir**.</span><span class="sxs-lookup"><span data-stu-id="96390-759">For example, we need to assign an IP address (in our example, **10.0.0.42**) for the cluster virtual host name **pr1-ascs-vir**.</span></span>

  ![Figur 23: I dialogrutan Egenskaper ändra IP-adressen][sap-ha-guide-figure-3012]

  <span data-ttu-id="96390-761">_**Figur 23:** i den **egenskaper** dialogrutan Ändra IP-adress_</span><span class="sxs-lookup"><span data-stu-id="96390-761">_**Figure 23:** In the **Properties** dialog box, change the IP address_</span></span>

  ![Figur 24: Tilldela IP-adress som är reserverad för klustret][sap-ha-guide-figure-3013]

  <span data-ttu-id="96390-763">_**Figur 24:** tilldela IP-adress som är reserverad för klustret_</span><span class="sxs-lookup"><span data-stu-id="96390-763">_**Figure 24:** Assign the IP address that is reserved for the cluster_</span></span>

6.  <span data-ttu-id="96390-764">Placera det virtuella värden klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="96390-764">Bring the cluster virtual host name online.</span></span>

  ![Bild 25: Core klustertjänsten är igång och körs och med rätt IP-adress][sap-ha-guide-figure-3014]

  <span data-ttu-id="96390-766">_**Bild 25:** core klustertjänsten är igång och körs och med rätt IP-adress_</span><span class="sxs-lookup"><span data-stu-id="96390-766">_**Figure 25:** Cluster core service is up and running, and with the correct IP address_</span></span>

7.  <span data-ttu-id="96390-767">Lägg till den andra noden i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-767">Add the second cluster node.</span></span>

  <span data-ttu-id="96390-768">Nu när klustertjänsten kärnor är igång, kan du lägga till den andra noden i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-768">Now that the core cluster service is up and running, you can add the second cluster node.</span></span>

  ![Bild 26: Lägga till den andra noden i klustret][sap-ha-guide-figure-3015]

  <span data-ttu-id="96390-770">_**Bild 26:** lägga till den andra noden i klustret_</span><span class="sxs-lookup"><span data-stu-id="96390-770">_**Figure 26:** Add the second cluster node_</span></span>

8.  <span data-ttu-id="96390-771">Ange ett namn för den andra nod klustervärden.</span><span class="sxs-lookup"><span data-stu-id="96390-771">Enter a name for the second cluster node host.</span></span>

  ![Bild 27: Ange andra värden klusternodnamnet][sap-ha-guide-figure-3016]

  <span data-ttu-id="96390-773">_**Bild 27:** ange andra värden klusternodnamnet_</span><span class="sxs-lookup"><span data-stu-id="96390-773">_**Figure 27:** Enter the second cluster node host name_</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="96390-774">Kontrollera att den **lägga till alla tillgängliga lagringsenheter i klustret** kryssrutan är **inte** valda.</span><span class="sxs-lookup"><span data-stu-id="96390-774">Be sure that the **Add all eligible storage to the cluster** check box is **NOT** selected.</span></span>  
  >
  >

  ![Bild 28: Markera inte kryssrutan][sap-ha-guide-figure-3017]

  <span data-ttu-id="96390-776">_**Bild 28:** gör **inte** Markera kryssrutan_</span><span class="sxs-lookup"><span data-stu-id="96390-776">_**Figure 28:** Do **not** select the check box_</span></span>

  <span data-ttu-id="96390-777">Du kan ignorera varningar om kvorum och diskar.</span><span class="sxs-lookup"><span data-stu-id="96390-777">You can ignore warnings about quorum and disks.</span></span> <span data-ttu-id="96390-778">Du måste ange kvorum och dela disken senare, enligt beskrivningen i [installerar SIOS DataKeeper Cluster Edition för SAP ASCS/SCS klusterdisk resursen][sap-ha-guide-8.12.3].</span><span class="sxs-lookup"><span data-stu-id="96390-778">You'll set the quorum and share the disk later, as described in [Installing SIOS DataKeeper Cluster Edition for SAP ASCS/SCS cluster share disk][sap-ha-guide-8.12.3].</span></span>

  ![Bild 29: Ignorera varningar om disken kvorum][sap-ha-guide-figure-3018]

  <span data-ttu-id="96390-780">_**Bild 29:** Ignorera varningar om disken kvorum_</span><span class="sxs-lookup"><span data-stu-id="96390-780">_**Figure 29:** Ignore warnings about the disk quorum_</span></span>


#### <span data-ttu-id="96390-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Konfigurera ett filresursvittne för kluster</span><span class="sxs-lookup"><span data-stu-id="96390-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Configure a cluster file share witness</span></span>

<span data-ttu-id="96390-782">När du konfigurerar ett kluster filresursvittne inkluderar dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="96390-782">Configuring a cluster file share witness involves these tasks:</span></span>

- <span data-ttu-id="96390-783">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="96390-783">Creating a file share</span></span>
- <span data-ttu-id="96390-784">Ange filen filresurs vittne kvorum i hanteraren för redundanskluster</span><span class="sxs-lookup"><span data-stu-id="96390-784">Setting the file share witness quorum in Failover Cluster Manager</span></span>

##### <span data-ttu-id="96390-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="96390-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Create a file share</span></span>

1.  <span data-ttu-id="96390-786">Välj ett filresursvittne i stället för en kvorumdisk.</span><span class="sxs-lookup"><span data-stu-id="96390-786">Select a file share witness instead of a quorum disk.</span></span> <span data-ttu-id="96390-787">SIOS DataKeeper stöder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="96390-787">SIOS DataKeeper supports this option.</span></span>

  <span data-ttu-id="96390-788">I exemplen i den här artikeln är filresursvittnet på Active Directory/DNS-server som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-788">In the examples in this article, the file share witness is on the Active Directory/DNS server that is running in Azure.</span></span> <span data-ttu-id="96390-789">Filresursvittnet kallas **domcontr 0**.</span><span class="sxs-lookup"><span data-stu-id="96390-789">The file share witness is called **domcontr-0**.</span></span> <span data-ttu-id="96390-790">Eftersom du skulle ha konfigurerat en VPN-anslutning till Azure (via plats-till-plats-VPN eller Azure ExpressRoute) kan dela din Active Directory/DNS-tjänsten är lokalt och inte är lämpligt att köra en fil vittne.</span><span class="sxs-lookup"><span data-stu-id="96390-790">Because you would have configured a VPN connection to Azure (via Site-to-Site VPN or Azure ExpressRoute), your Active Directory/DNS service is on-premises and isn't suitable to run a file share witness.</span></span>

  > [!NOTE]
  > <span data-ttu-id="96390-791">Om Active Directory/DNS-tjänsten körs bara lokalt, inte konfigurera din filresursvittne på Active Directory/DNS-Windows-operativsystemet som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="96390-791">If your Active Directory/DNS service runs only on-premises, don't configure your file share witness on the Active Directory/DNS Windows operating system that is running on-premises.</span></span> <span data-ttu-id="96390-792">Nätverksfördröjningen mellan klusternoder som körs i Azure och Active Directory/DNS lokalt kan vara för stort och orsaka problem med nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="96390-792">Network latency between cluster nodes running in Azure and Active Directory/DNS on-premises might be too large and cause connectivity issues.</span></span> <span data-ttu-id="96390-793">Se till att konfigurera filresursvittnet på en virtuell Azure-dator som kör nära noden i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-793">Be sure to configure the file share witness on an Azure virtual machine that is running close to the cluster node.</span></span>  
  >
  >

  <span data-ttu-id="96390-794">Kvorum-enhet behöver minst 1 024 MB ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="96390-794">The quorum drive needs at least 1,024 MB of free space.</span></span> <span data-ttu-id="96390-795">Vi rekommenderar 2 048 MB ledigt utrymme för enheten kvorum.</span><span class="sxs-lookup"><span data-stu-id="96390-795">We recommend 2,048 MB of free space for the quorum drive.</span></span>

2.  <span data-ttu-id="96390-796">Lägg till klusternamnobjektet.</span><span class="sxs-lookup"><span data-stu-id="96390-796">Add the cluster name object.</span></span>

  ![Bild 30: Tilldela behörigheter för resurs för klusternamnobjektet][sap-ha-guide-figure-3019]

  <span data-ttu-id="96390-798">_**Bild 30:** tilldela behörigheter för resurs för klusternamnobjektet_</span><span class="sxs-lookup"><span data-stu-id="96390-798">_**Figure 30:** Assign the permissions on the share for the cluster name object_</span></span>

  <span data-ttu-id="96390-799">Se till att behörigheterna som inkluderar behörighet att ändra data i resursen för klusternamnobjektet (i vårt exempel **pr1-ascs-vir$**).</span><span class="sxs-lookup"><span data-stu-id="96390-799">Be sure that the permissions include the authority to change data in the share for the cluster name object (in our example, **pr1-ascs-vir$**).</span></span>

3.  <span data-ttu-id="96390-800">Om du vill lägga till klusternamnobjektet i listan, Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="96390-800">To add the cluster name object to the list, select **Add**.</span></span> <span data-ttu-id="96390-801">Ändra filtreringen för att söka efter datorobjekt, förutom de som visas i bild 31.</span><span class="sxs-lookup"><span data-stu-id="96390-801">Change the filter to check for computer objects, in addition to those shown in Figure 31.</span></span>

  ![Bild 31: Ändra objekttyper för att inkludera datorer][sap-ha-guide-figure-3020]

  <span data-ttu-id="96390-803">_**Bild 31:** ändra objekttyper för att inkludera datorer_</span><span class="sxs-lookup"><span data-stu-id="96390-803">_**Figure 31:** Change the Object Types to include computers_</span></span>

  ![Bild 32: Markera kryssrutan datorer][sap-ha-guide-figure-3021]

  <span data-ttu-id="96390-805">_**Bild 32:** markerar du den **datorer** kryssruta_</span><span class="sxs-lookup"><span data-stu-id="96390-805">_**Figure 32:** Select the **Computers** check box_</span></span>

4.  <span data-ttu-id="96390-806">Ange klusternamnobjektet som visas i bild 31.</span><span class="sxs-lookup"><span data-stu-id="96390-806">Enter the cluster name object as shown in Figure 31.</span></span> <span data-ttu-id="96390-807">Eftersom posten redan har skapats kan du ändra behörigheter, som visas i bild 30.</span><span class="sxs-lookup"><span data-stu-id="96390-807">Because the record has already been created, you can change the permissions, as shown in Figure 30.</span></span>

5.  <span data-ttu-id="96390-808">Välj den **säkerhet** för resursen och ange sedan mer detaljerade behörigheter för klusternamnobjektet.</span><span class="sxs-lookup"><span data-stu-id="96390-808">Select the **Security** tab of the share, and then set more detailed permissions for the cluster name object.</span></span>

  ![Bild 33: Ange säkerhetsattribut för klusterobjektet namn på filen resursen kvorum][sap-ha-guide-figure-3022]

  <span data-ttu-id="96390-810">_**Bild 33:** ange säkerhetsattribut för klusterobjektet namn på filen resursen kvorum_</span><span class="sxs-lookup"><span data-stu-id="96390-810">_**Figure 33:** Set the security attributes for the cluster name object on the file share quorum_</span></span>

##### <span data-ttu-id="96390-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Ange filen filresurs vittne kvorum i hanteraren för redundanskluster</span><span class="sxs-lookup"><span data-stu-id="96390-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Set the file share witness quorum in Failover Cluster Manager</span></span>

1.  <span data-ttu-id="96390-812">Öppna den kvorum inställningen guiden Konfigurera.</span><span class="sxs-lookup"><span data-stu-id="96390-812">Open the Configure Quorum Setting Wizard.</span></span>

  ![Bild 34: Starta konfigurera inställningen guiden klusterkvorum][sap-ha-guide-figure-3023]

  <span data-ttu-id="96390-814">_**Bild 34:** börja konfigurera inställningen guiden klusterkvorum_</span><span class="sxs-lookup"><span data-stu-id="96390-814">_**Figure 34:** Start the Configure Cluster Quorum Setting Wizard_</span></span>

2.  <span data-ttu-id="96390-815">På den **Välj kvorumkonfigurationen** väljer **Välj kvorumvittnet**.</span><span class="sxs-lookup"><span data-stu-id="96390-815">On the **Select Quorum Configuration** page, select **Select the quorum witness**.</span></span>

  ![Bild 35: Kvorumkonfigurationer som du kan välja mellan][sap-ha-guide-figure-3024]

  <span data-ttu-id="96390-817">_**Bild 35:** kvorumkonfigurationer som du kan välja mellan_</span><span class="sxs-lookup"><span data-stu-id="96390-817">_**Figure 35:** Quorum configurations you can choose from_</span></span>

3.  <span data-ttu-id="96390-818">På den **Välj Kvorumvittne** väljer **konfigurera ett filresursvittne**.</span><span class="sxs-lookup"><span data-stu-id="96390-818">On the **Select Quorum Witness** page, select **Configure a file share witness**.</span></span>

  ![Bild 36: Välj filresursvittnet][sap-ha-guide-figure-3025]

  <span data-ttu-id="96390-820">_**Bild 36:** Välj filresursvittnet_</span><span class="sxs-lookup"><span data-stu-id="96390-820">_**Figure 36:** Select the file share witness_</span></span>

4.  <span data-ttu-id="96390-821">Ange UNC-sökvägen till filresursen (i vårt exempel \\domcontr 0\FSW).</span><span class="sxs-lookup"><span data-stu-id="96390-821">Enter the UNC path to the file share (in our example, \\domcontr-0\FSW).</span></span> <span data-ttu-id="96390-822">Om du vill se en lista över ändringar kan du markera **nästa**.</span><span class="sxs-lookup"><span data-stu-id="96390-822">To see a list of the changes you can make, select **Next**.</span></span>

  ![Bild 37: Definiera filresursplats för resursen vittne][sap-ha-guide-figure-3026]

  <span data-ttu-id="96390-824">_**Bild 37:** definiera filresursplats för resursen vittne_</span><span class="sxs-lookup"><span data-stu-id="96390-824">_**Figure 37:** Define the file share location for the witness share_</span></span>

5.  <span data-ttu-id="96390-825">Välj önskade ändringar och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="96390-825">Select the changes you want, and then select **Next**.</span></span> <span data-ttu-id="96390-826">Du måste konfigurera om klusterkonfigurationen har enligt figur 38.</span><span class="sxs-lookup"><span data-stu-id="96390-826">You need to successfully reconfigure the cluster configuration as shown in Figure 38.</span></span>  

  ![Bild 38: Bekräfta att klustret har konfigurerats om][sap-ha-guide-figure-3027]

  <span data-ttu-id="96390-828">_**Bild 38:** bekräfta att klustret har konfigurerats om_</span><span class="sxs-lookup"><span data-stu-id="96390-828">_**Figure 38:** Confirmation that you've reconfigured the cluster_</span></span>

<span data-ttu-id="96390-829">När du har installerat Windows-redundanskluster har förändringar som måste göras för vissa tröskelvärden för att anpassa redundans identifiering till villkoren i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-829">After installing the Windows Failover Cluster successfully, changes need to be made to some thresholds to adapt failover detection to conditions in Azure.</span></span> <span data-ttu-id="96390-830">Parametrarna för att ändra dokumenteras i den här bloggen: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/.</span><span class="sxs-lookup"><span data-stu-id="96390-830">The parameters to be changed are documented in this blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ .</span></span> <span data-ttu-id="96390-831">Förutsatt att din två virtuella datorer som bygger Windows klusterkonfigurationen för ASCS/SCS är i samma undernät, måste följande parametrar ändras till dessa värden:</span><span class="sxs-lookup"><span data-stu-id="96390-831">Assuming that your two VMs that build the Windows Cluster Configuration for ASCS/SCS are in the same SubNet, the following parameters need to be changed to these values:</span></span>
- <span data-ttu-id="96390-832">SameSubNetDelay = 2</span><span class="sxs-lookup"><span data-stu-id="96390-832">SameSubNetDelay = 2</span></span>
- <span data-ttu-id="96390-833">SameSubNetThreshold = 15</span><span class="sxs-lookup"><span data-stu-id="96390-833">SameSubNetThreshold = 15</span></span>

<span data-ttu-id="96390-834">Dessa inställningar har testats med kunder och tillhandahålls en bra kompromiss om du vill bli motståndskraftiga på ena sidan.</span><span class="sxs-lookup"><span data-stu-id="96390-834">These settings were tested with customers and provided a good compromise to be resilient enough on the one side.</span></span> <span data-ttu-id="96390-835">Å andra sidan dessa inställningar ger snabb tillräckligt med redundans i verkliga fel vid SAP programvara eller nod och VM-fel.</span><span class="sxs-lookup"><span data-stu-id="96390-835">On the other hand those settings were providing fast enough failover in real error conditions on SAP software or node/VM failure.</span></span> 

### <span data-ttu-id="96390-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Installera SIOS DataKeeper Cluster Edition för SAP ASCS/SCS resursen klusterdisken</span><span class="sxs-lookup"><span data-stu-id="96390-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Install SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk</span></span>

<span data-ttu-id="96390-837">Nu har du en fungerande konfiguration för Windows Server Failover Clustering i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-837">You now have a working Windows Server Failover Clustering configuration in Azure.</span></span> <span data-ttu-id="96390-838">Men om du vill installera en SAP ASCS/SCS-instans, måste en delad disk-resurs.</span><span class="sxs-lookup"><span data-stu-id="96390-838">But, to install an SAP ASCS/SCS instance, you need a shared disk resource.</span></span> <span data-ttu-id="96390-839">Du kan inte skapa de delade diskresurserna som du behöver i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-839">You cannot create the shared disk resources you need in Azure.</span></span> <span data-ttu-id="96390-840">SIOS DataKeeper Cluster Edition är en lösning för från tredje part som du kan använda för att skapa delade diskresurserna.</span><span class="sxs-lookup"><span data-stu-id="96390-840">SIOS DataKeeper Cluster Edition is a third-party solution you can use to create shared disk resources.</span></span>

<span data-ttu-id="96390-841">Installera SIOS DataKeeper Cluster Edition för SAP ASCS/SCS resursen klusterdisken omfattar dessa aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="96390-841">Installing SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk involves these tasks:</span></span>

- <span data-ttu-id="96390-842">Lägga till .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="96390-842">Adding the .NET Framework 3.5</span></span>
- <span data-ttu-id="96390-843">Installera SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="96390-843">Installing SIOS DataKeeper</span></span>
- <span data-ttu-id="96390-844">Ställa in SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="96390-844">Setting up SIOS DataKeeper</span></span>

#### <span data-ttu-id="96390-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Lägg till .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="96390-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Add the .NET Framework 3.5</span></span>
<span data-ttu-id="96390-846">Microsoft .NET Framework 3.5 är inte automatiskt aktiverats eller installerats på Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="96390-846">The Microsoft .NET Framework 3.5 isn't automatically activated or installed on Windows Server 2012 R2.</span></span> <span data-ttu-id="96390-847">Eftersom SIOS DataKeeper kräver .NET Framework på alla noder som du installerar DataKeeper på, måste du installera .NET Framework 3.5 på gästoperativsystemet för alla virtuella datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-847">Because SIOS DataKeeper requires the .NET Framework to be on all nodes that you install DataKeeper on, you must install the .NET Framework 3.5 on the guest operating system of all virtual machines in the cluster.</span></span>

<span data-ttu-id="96390-848">Det finns två sätt att lägga till .NET Framework 3.5:</span><span class="sxs-lookup"><span data-stu-id="96390-848">There are two ways to add the .NET Framework 3.5:</span></span>

- <span data-ttu-id="96390-849">Använd guiden Lägg till roller och funktioner i Windows som visas i bild 39.</span><span class="sxs-lookup"><span data-stu-id="96390-849">Use the Add Roles and Features Wizard in Windows as shown in Figure 39.</span></span>

  ![Bild 39: Installera .NET Framework 3.5 med hjälp av guiden Lägg till roller och funktioner][sap-ha-guide-figure-3028]

  <span data-ttu-id="96390-851">_**Bild 39:** installera .NET Framework 3.5 med hjälp av guiden Lägg till roller och funktioner_</span><span class="sxs-lookup"><span data-stu-id="96390-851">_**Figure 39:** Install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

  ![Bild 40: Installationen förloppsindikator när du installerar .NET Framework 3.5 med hjälp av guiden Lägg till roller och funktioner][sap-ha-guide-figure-3029]

  <span data-ttu-id="96390-853">_**Bild 40:** Installation förloppsindikator när du installerar .NET Framework 3.5 med hjälp av guiden Lägg till roller och funktioner_</span><span class="sxs-lookup"><span data-stu-id="96390-853">_**Figure 40:** Installation progress bar when you install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

- <span data-ttu-id="96390-854">Använd kommandoradsverktyget dism.exe.</span><span class="sxs-lookup"><span data-stu-id="96390-854">Use the command-line tool dism.exe.</span></span> <span data-ttu-id="96390-855">För den här typen av installation som du behöver åtkomst till katalogen SxS på installationsmediet för Windows.</span><span class="sxs-lookup"><span data-stu-id="96390-855">For this type of installation, you need to access the SxS directory on the Windows installation media.</span></span> <span data-ttu-id="96390-856">Skriv följande vid en upphöjd kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="96390-856">At an elevated command prompt, type:</span></span>

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <span data-ttu-id="96390-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a>Installera SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="96390-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper</span></span>

<span data-ttu-id="96390-858">Installera SIOS DataKeeper Cluster Edition på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-858">Install SIOS DataKeeper Cluster Edition on each node in the cluster.</span></span> <span data-ttu-id="96390-859">För att skapa virtuella delad lagring med SIOS DataKeeper, skapa en synkroniserad spegling och simulera delad klusterlagring.</span><span class="sxs-lookup"><span data-stu-id="96390-859">To create virtual shared storage with SIOS DataKeeper, create a synced mirror and then simulate cluster shared storage.</span></span>

<span data-ttu-id="96390-860">Innan du installerar programvaran SIOS skapa domänanvändaren **DataKeeperSvc**.</span><span class="sxs-lookup"><span data-stu-id="96390-860">Before you install the SIOS software, create the domain user **DataKeeperSvc**.</span></span>

> [!NOTE]
> <span data-ttu-id="96390-861">Lägg till den **DataKeeperSvc** användare till den **lokal administratör** på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="96390-861">Add the **DataKeeperSvc** user to the **Local Administrator** group on both cluster nodes.</span></span>
>
>

<span data-ttu-id="96390-862">Installera SIOS DataKeeper:</span><span class="sxs-lookup"><span data-stu-id="96390-862">To install SIOS DataKeeper:</span></span>

1.  <span data-ttu-id="96390-863">Installera den SIOS på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="96390-863">Install the SIOS software on both cluster nodes.</span></span>

  ![SIOS installer][sap-ha-guide-figure-3030]

  ![Bild 41: Första sidan i SIOS DataKeeper-installation][sap-ha-guide-figure-3031]

  <span data-ttu-id="96390-866">_**Bild 41:** första sidan i SIOS DataKeeper-installation_</span><span class="sxs-lookup"><span data-stu-id="96390-866">_**Figure 41:** First page of the SIOS DataKeeper installation_</span></span>

2.  <span data-ttu-id="96390-867">Välj i dialogrutan som visas i bild 42 **Ja**.</span><span class="sxs-lookup"><span data-stu-id="96390-867">In the dialog box shown in Figure 42, select **Yes**.</span></span>

  ![Bild 42: DataKeeper informerar dig om att en tjänst kommer att inaktiveras][sap-ha-guide-figure-3032]

  <span data-ttu-id="96390-869">_**Bild 42:** DataKeeper informerar dig om att en tjänst kommer att inaktiveras_</span><span class="sxs-lookup"><span data-stu-id="96390-869">_**Figure 42:** DataKeeper informs you that a service will be disabled_</span></span>

3.  <span data-ttu-id="96390-870">I dialogrutan som visas i bild 43 rekommenderar vi att du väljer **domän eller Server konto**.</span><span class="sxs-lookup"><span data-stu-id="96390-870">In the dialog box shown in Figure 43, we recommend that you select **Domain or Server account**.</span></span>

  ![Bild 43: Användarens val för SIOS DataKeeper][sap-ha-guide-figure-3033]

  <span data-ttu-id="96390-872">_**Bild 43:** användarens val för SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="96390-872">_**Figure 43:** User selection for SIOS DataKeeper_</span></span>

4.  <span data-ttu-id="96390-873">Ange domänanvändarnamn och lösenord som du skapade för SIOS DataKeeper.</span><span class="sxs-lookup"><span data-stu-id="96390-873">Enter the domain account user name and passwords that you created for SIOS DataKeeper.</span></span>

  ![Bild 44: Ange användarnamn och lösenord för SIOS DataKeeper-installation][sap-ha-guide-figure-3034]

  <span data-ttu-id="96390-875">_**Bild 44:** ange användarnamn och lösenord för SIOS DataKeeper installationen_</span><span class="sxs-lookup"><span data-stu-id="96390-875">_**Figure 44:** Enter the domain user name and password for the SIOS DataKeeper installation_</span></span>

5.  <span data-ttu-id="96390-876">Installera licensnyckeln för SIOS DataKeeper-instans som visas i bild 45.</span><span class="sxs-lookup"><span data-stu-id="96390-876">Install the license key for your SIOS DataKeeper instance as shown in Figure 45.</span></span>

  ![Bild 45: Ange licensnyckeln SIOS DataKeeper][sap-ha-guide-figure-3035]

  <span data-ttu-id="96390-878">_**Bild 45:** Ange licensnyckeln SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="96390-878">_**Figure 45:** Enter your SIOS DataKeeper license key_</span></span>

6.  <span data-ttu-id="96390-879">När du uppmanas starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="96390-879">When prompted, restart the virtual machine.</span></span>

#### <span data-ttu-id="96390-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Ställ in SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="96390-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Set up SIOS DataKeeper</span></span>

<span data-ttu-id="96390-881">När du installerar SIOS DataKeeper på båda noderna, måste du starta konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="96390-881">After you install SIOS DataKeeper on both nodes, you need to start the configuration.</span></span> <span data-ttu-id="96390-882">Målet med konfigurationen är att ha synkron replikering mellan de ytterligare virtuella hårddiskar anslutna till var och en av de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="96390-882">The goal of the configuration is to have synchronous data replication between the additional VHDs attached to each of the virtual machines.</span></span>

1.  <span data-ttu-id="96390-883">Starta verktyget DataKeeper hantering och konfiguration och välj sedan **ansluta servern**.</span><span class="sxs-lookup"><span data-stu-id="96390-883">Start the DataKeeper Management and Configuration tool, and then select **Connect Server**.</span></span> <span data-ttu-id="96390-884">(I figur 46 det här alternativet är i en röd cirkel.)</span><span class="sxs-lookup"><span data-stu-id="96390-884">(In Figure 46, this option is circled in red.)</span></span>

  ![Bild 46: SIOS DataKeeper hantering och verktyg][sap-ha-guide-figure-3036]

  <span data-ttu-id="96390-886">_**Bild 46:** SIOS DataKeeper hantering och konfiguration av verktyget_</span><span class="sxs-lookup"><span data-stu-id="96390-886">_**Figure 46:** SIOS DataKeeper Management and Configuration tool_</span></span>

2.  <span data-ttu-id="96390-887">Ange namn eller TCP/IP-adressen för den första noden verktyget hantering och konfiguration ska ansluta till, och i andra steget, den andra noden.</span><span class="sxs-lookup"><span data-stu-id="96390-887">Enter the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and, in a second step, the second node.</span></span>

  ![Bild 47: Infoga namnet eller TCP/IP-adressen för den första noden hanteringen och konfigurationsverktyget ska ansluta till och i andra steget, den andra noden][sap-ha-guide-figure-3037]

  <span data-ttu-id="96390-889">_**Bild 47:** infoga namn eller TCP/IP-adress för den första noden verktyget hantering och konfiguration ska ansluta till och i andra steget, den andra noden_</span><span class="sxs-lookup"><span data-stu-id="96390-889">_**Figure 47:** Insert the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and in a second step, the second node_</span></span>

3.  <span data-ttu-id="96390-890">Skapa jobbet replikering mellan två noder.</span><span class="sxs-lookup"><span data-stu-id="96390-890">Create the replication job between the two nodes.</span></span>

  ![Bild 48: Skapa ett jobb för replikering][sap-ha-guide-figure-3038]

  <span data-ttu-id="96390-892">_**Bild 48:** skapa ett jobb för replikering_</span><span class="sxs-lookup"><span data-stu-id="96390-892">_**Figure 48:** Create a replication job_</span></span>

  <span data-ttu-id="96390-893">En guide hjälper dig att skapa ett jobb för replikering.</span><span class="sxs-lookup"><span data-stu-id="96390-893">A wizard guides you through the process of creating a replication job.</span></span>
4.  <span data-ttu-id="96390-894">Definiera namn, TCP/IP-adress och diskvolymen till Källnoden.</span><span class="sxs-lookup"><span data-stu-id="96390-894">Define the name, TCP/IP address, and disk volume of the source node.</span></span>

  ![Bild 49: Ange namnet på jobbet replikering][sap-ha-guide-figure-3039]

  <span data-ttu-id="96390-896">_**Bild 49:** definiera namnet på jobbet replikering_</span><span class="sxs-lookup"><span data-stu-id="96390-896">_**Figure 49:** Define the name of the replication job_</span></span>

  ![Bild 50: Definiera grundläggande data för noden som ska vara aktuella Källnoden][sap-ha-guide-figure-3040]

  <span data-ttu-id="96390-898">_**Bild 50:** definiera grundläggande data för noden som ska vara aktuella Källnoden_</span><span class="sxs-lookup"><span data-stu-id="96390-898">_**Figure 50:** Define the base data for the node, which should be the current source node_</span></span>

5.  <span data-ttu-id="96390-899">Definiera namn, TCP/IP-adress och volymen av målnoden.</span><span class="sxs-lookup"><span data-stu-id="96390-899">Define the name, TCP/IP address, and disk volume of the target node.</span></span>

  ![Bild av 51: Definiera grundläggande data för noden som ska vara den aktuella målnoden][sap-ha-guide-figure-3041]

  <span data-ttu-id="96390-901">_**Bild av 51:** definiera grundläggande data för noden som ska vara den aktuella målnoden_</span><span class="sxs-lookup"><span data-stu-id="96390-901">_**Figure 51:** Define the base data for the node, which should be the current target node_</span></span>

6.  <span data-ttu-id="96390-902">Definiera komprimeringsalgoritmer.</span><span class="sxs-lookup"><span data-stu-id="96390-902">Define the compression algorithms.</span></span> <span data-ttu-id="96390-903">I vårt exempel rekommenderar vi att du komprimera dataströmmen för replikering.</span><span class="sxs-lookup"><span data-stu-id="96390-903">In our example, we recommend that you compress the replication stream.</span></span> <span data-ttu-id="96390-904">Särskilt i omsynkroniseringen situationer minskar komprimeringen av replikering dataströmmen kraftigt omsynkroniseringen tid.</span><span class="sxs-lookup"><span data-stu-id="96390-904">Especially in resynchronization situations, the compression of the replication stream dramatically reduces resynchronization time.</span></span> <span data-ttu-id="96390-905">Observera att komprimering används processor och RAM resurser för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="96390-905">Note that compression uses the CPU and RAM resources of a virtual machine.</span></span> <span data-ttu-id="96390-906">När hastigheten med komprimering ökar, ökar mängden processorresurser som används.</span><span class="sxs-lookup"><span data-stu-id="96390-906">As the compression rate increases, so does the volume of CPU resources used.</span></span> <span data-ttu-id="96390-907">Du kan också ändra denna inställning senare.</span><span class="sxs-lookup"><span data-stu-id="96390-907">You also can adjust this setting later.</span></span>

7.  <span data-ttu-id="96390-908">En annan inställning som du behöver kontrollera är om replikeringen sker synkront eller asynkront.</span><span class="sxs-lookup"><span data-stu-id="96390-908">Another setting you need to check is whether the replication occurs asynchronously or synchronously.</span></span> <span data-ttu-id="96390-909">*När du skyddar SAP ASCS/SCS konfigurationer, måste du använda synkron replikering*.</span><span class="sxs-lookup"><span data-stu-id="96390-909">*When you protect SAP ASCS/SCS configurations, you must use synchronous replication*.</span></span>  

  ![Bild 52: Definiera replikeringsinformation][sap-ha-guide-figure-3042]

  <span data-ttu-id="96390-911">_**Bild 52:** definiera replikeringsinformation_</span><span class="sxs-lookup"><span data-stu-id="96390-911">_**Figure 52:** Define replication details_</span></span>

8.  <span data-ttu-id="96390-912">Definiera om volymen som replikeras av replikering jobbet ska representeras som en delad disk på en Windows Server Failover Clustering klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="96390-912">Define whether the volume that is replicated by the replication job should be represented to a Windows Server Failover Clustering cluster configuration as a shared disk.</span></span> <span data-ttu-id="96390-913">SAP ASCS/SCS-konfiguration, markera **Ja** så att Windows-kluster ser replikerade volymen som en delad disk som kan användas som en volym i klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-913">For the SAP ASCS/SCS configuration, select **Yes** so that the Windows cluster sees the replicated volume as a shared disk that it can use as a cluster volume.</span></span>

  ![Bild 53: Välj Ja om du vill ange replikerade volymen som en klustervolym][sap-ha-guide-figure-3043]

  <span data-ttu-id="96390-915">_**Bild 53:** Välj **Ja** ange replikerade volymen som en klustervolym_</span><span class="sxs-lookup"><span data-stu-id="96390-915">_**Figure 53:** Select **Yes** to set the replicated volume as a cluster volume_</span></span>

  <span data-ttu-id="96390-916">När volymen har skapats, verktyget DataKeeper hantering och konfiguration som visar att jobbet replikering är aktiv.</span><span class="sxs-lookup"><span data-stu-id="96390-916">After the volume is created, the DataKeeper Management and Configuration tool shows that the replication job is active.</span></span>

  ![Bild 54: DataKeeper synkroniserad spegling för SAP ASCS/SCS dela disken är aktiv][sap-ha-guide-figure-3044]

  <span data-ttu-id="96390-918">_**Bild 54:** DataKeeper synkroniserad spegling för SAP ASCS/SCS dela disken är aktiv_</span><span class="sxs-lookup"><span data-stu-id="96390-918">_**Figure 54:** DataKeeper synchronous mirroring for the SAP ASCS/SCS share disk is active_</span></span>

  <span data-ttu-id="96390-919">Hanteraren för redundanskluster innehåller nu disk som en DataKeeper disk som visas i figur 55.</span><span class="sxs-lookup"><span data-stu-id="96390-919">Failover Cluster Manager now shows the disk as a DataKeeper disk, as shown in Figure 55.</span></span>

  ![Bild 55: Klusterhanterare för växling vid fel visas den disk som DataKeeper replikeras][sap-ha-guide-figure-3045]

  <span data-ttu-id="96390-921">_**Bild 55:** Klusterhanteraren visar disken som DataKeeper replikeras_</span><span class="sxs-lookup"><span data-stu-id="96390-921">_**Figure 55:** Failover Cluster Manager shows the disk that DataKeeper replicated_</span></span>

## <span data-ttu-id="96390-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Installera SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="96390-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Install the SAP NetWeaver system</span></span>

<span data-ttu-id="96390-923">Vi kommer inte beskriver DBMS-inställningarna eftersom inställningar varierar beroende på DBMS-system som du använder.</span><span class="sxs-lookup"><span data-stu-id="96390-923">We won’t describe the DBMS setup because setups vary depending on the DBMS system you use.</span></span> <span data-ttu-id="96390-924">Men förutsätter vi att få hög tillgänglighet med DBMS behandlas med funktioner som har stöd för olika DBMS-leverantörer för Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-924">However, we assume that high-availability concerns with the DBMS are addressed with the functionalities the different DBMS vendors support for Azure.</span></span> <span data-ttu-id="96390-925">Till exempel alltid på eller databasspegling för SQL Server och Oracle Data Guard för Oracle-databaser.</span><span class="sxs-lookup"><span data-stu-id="96390-925">For example, Always On or database mirroring for SQL Server, and Oracle Data Guard for Oracle databases.</span></span> <span data-ttu-id="96390-926">I scenariot som vi använder i den här artikeln inte vi lägga till mer skydd DBMS.</span><span class="sxs-lookup"><span data-stu-id="96390-926">In the scenario we use in this article, we didn't add more protection to the DBMS.</span></span>

<span data-ttu-id="96390-927">Det finns inga särskilda överväganden när olika DBMS tjänster interagera med den här typen av klustrad SAP ASCS/SCS konfiguration i Azure.</span><span class="sxs-lookup"><span data-stu-id="96390-927">There are no special considerations when different DBMS services interact with this kind of clustered SAP ASCS/SCS configuration in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="96390-928">Installationsproceduren för SAP NetWeaver ABAP system, Java-system och ABAP + Java system är nästan identiska.</span><span class="sxs-lookup"><span data-stu-id="96390-928">The installation procedures of SAP NetWeaver ABAP systems, Java systems, and ABAP+Java systems are almost identical.</span></span> <span data-ttu-id="96390-929">Den viktigaste skillnaden är att ett SAP ABAP system har en ASCS-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-929">The most significant difference is that an SAP ABAP system has one ASCS instance.</span></span> <span data-ttu-id="96390-930">SAP Java-systemet har en SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-930">The SAP Java system has one SCS instance.</span></span> <span data-ttu-id="96390-931">SAP ABAP + Java-systemet har en ASCS-instans och en SCS-instans som körs i gruppen samma Microsoft för failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="96390-931">The SAP ABAP+Java system has one ASCS instance and one SCS instance running in the same Microsoft failover cluster group.</span></span> <span data-ttu-id="96390-932">Installationen skillnader för varje SAP NetWeaver installationen stack anges explicit.</span><span class="sxs-lookup"><span data-stu-id="96390-932">Any installation differences for each SAP NetWeaver installation stack are explicitly mentioned.</span></span> <span data-ttu-id="96390-933">Du kan anta att alla delar är samma.</span><span class="sxs-lookup"><span data-stu-id="96390-933">You can assume that all other parts are the same.</span></span>  
>
>

### <span data-ttu-id="96390-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Installera SAP med en hög tillgänglighet ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Install SAP with a high-availability ASCS/SCS instance</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96390-935">Glöm inte att placera växlingsfilen på DataKeeper speglade volymer.</span><span class="sxs-lookup"><span data-stu-id="96390-935">Be sure not to place your page file on DataKeeper mirrored volumes.</span></span> <span data-ttu-id="96390-936">DataKeeper stöder inte speglade volymer.</span><span class="sxs-lookup"><span data-stu-id="96390-936">DataKeeper does not support mirrored volumes.</span></span> <span data-ttu-id="96390-937">Du kan lämna växlingsfilen på den tillfälliga enheten D av en virtuell Azure-dator, vilket är standard.</span><span class="sxs-lookup"><span data-stu-id="96390-937">You can leave your page file on the temporary drive D of an Azure virtual machine, which is the default.</span></span> <span data-ttu-id="96390-938">Om den inte redan finns där, flytta växlingsfilen till enhet D på ditt Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="96390-938">If it's not already there, move the Windows page file to drive D of your Azure virtual machine.</span></span>
>
>

<span data-ttu-id="96390-939">Dessa uppgifter omfattar att installera SAP med en hög tillgänglighet ASCS/SCS-instans:</span><span class="sxs-lookup"><span data-stu-id="96390-939">Installing SAP with a high-availability ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="96390-940">Skapa ett virtuellt värdnamn för den klustrade instansen SAP ASCS/SCS</span><span class="sxs-lookup"><span data-stu-id="96390-940">Creating a virtual host name for the clustered SAP ASCS/SCS instance</span></span>
- <span data-ttu-id="96390-941">Installera den första klusternoden SAP</span><span class="sxs-lookup"><span data-stu-id="96390-941">Installing the SAP first cluster node</span></span>
- <span data-ttu-id="96390-942">Ändra SAP-profilen för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="96390-942">Modifying the SAP profile of the ASCS/SCS instance</span></span>
- <span data-ttu-id="96390-943">Lägger till en avsökningsport</span><span class="sxs-lookup"><span data-stu-id="96390-943">Adding a probe port</span></span>
- <span data-ttu-id="96390-944">Öppna avsökningsport för Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="96390-944">Opening the Windows firewall probe port</span></span>

#### <span data-ttu-id="96390-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Skapa ett virtuellt värdnamn för den klustrade instansen SAP ASCS/SCS</span><span class="sxs-lookup"><span data-stu-id="96390-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Create a virtual host name for the clustered SAP ASCS/SCS instance</span></span>

1.  <span data-ttu-id="96390-946">Skapa en DNS-post för virtuella värdnamn för ASCS/SCS-instansen i Windows DNS-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="96390-946">In the Windows DNS manager, create a DNS entry for the virtual host name of the ASCS/SCS instance.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="96390-947">IP-adressen som du tilldelar virtuella värdnamn för ASCS/SCS-instansen måste vara samma som IP-adressen som du tilldelade till Azure belastningsutjämnare (**<*SID*> - lb - ascs **).</span><span class="sxs-lookup"><span data-stu-id="96390-947">The IP address that you assign to the virtual host name of the ASCS/SCS instance must be the same as the IP address that you assigned to Azure Load Balancer (**<*SID*>-lb-ascs**).</span></span>  
  >
  >

  <span data-ttu-id="96390-948">IP-adressen för den virtuella värdnamnet för SAP ASCS/SCS (**pr1-ascs-sap**) är samma som IP-adressen för Azure belastningsutjämnare (**pr1-lb-ascs**).</span><span class="sxs-lookup"><span data-stu-id="96390-948">The IP address of the virtual SAP ASCS/SCS host name (**pr1-ascs-sap**) is the same as the IP address of Azure Load Balancer (**pr1-lb-ascs**).</span></span>

  ![Bild 56: Definiera DNS-posten för SAP ASCS/SCS virtuell klusternamnet och TCP/IP-adress][sap-ha-guide-figure-3046]

  <span data-ttu-id="96390-950">_**Bild 56:** definiera DNS-posten för SAP ASCS/SCS virtuell klusternamnet och TCP/IP-adress_</span><span class="sxs-lookup"><span data-stu-id="96390-950">_**Figure 56:** Define the DNS entry for the SAP ASCS/SCS cluster virtual name and TCP/IP address_</span></span>

2.  <span data-ttu-id="96390-951">Ange IP-adress som tilldelats till namnet på virtuella värddatorn, välja **DNS-hanteraren** > **domän**.</span><span class="sxs-lookup"><span data-stu-id="96390-951">To define the IP address assigned to the virtual host name, select **DNS Manager** > **Domain**.</span></span>

  ![Bild 57: Nya virtuella namn och TCP/IP-adress för SAP ASCS/SCS klusterkonfiguration][sap-ha-guide-figure-3047]

  <span data-ttu-id="96390-953">_**Bild 57:** nya virtuella namn och TCP/IP-adress för SAP ASCS/SCS klusterkonfiguration_</span><span class="sxs-lookup"><span data-stu-id="96390-953">_**Figure 57:** New virtual name and TCP/IP address for SAP ASCS/SCS cluster configuration_</span></span>

#### <span data-ttu-id="96390-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Installera den första klusternoden SAP</span><span class="sxs-lookup"><span data-stu-id="96390-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Install the SAP first cluster node</span></span>

1.  <span data-ttu-id="96390-955">Kör det första klustra nod alternativet på klusternoden A. Till exempel på den **pr1-ascs-0** värden.</span><span class="sxs-lookup"><span data-stu-id="96390-955">Execute the first cluster node option on cluster node A. For example, on the **pr1-ascs-0** host.</span></span>
2.  <span data-ttu-id="96390-956">Om du vill behålla standard portar för den interna belastningsutjämnaren som Azure, väljer du:</span><span class="sxs-lookup"><span data-stu-id="96390-956">To keep the default ports for the Azure internal load balancer, select:</span></span>

  * <span data-ttu-id="96390-957">**ABAP system**: **ASCS** instansen nummer **00**</span><span class="sxs-lookup"><span data-stu-id="96390-957">**ABAP system**: **ASCS** instance number **00**</span></span>
  * <span data-ttu-id="96390-958">**Java system**: **SCS** instansen nummer **01**</span><span class="sxs-lookup"><span data-stu-id="96390-958">**Java system**: **SCS** instance number **01**</span></span>
  * <span data-ttu-id="96390-959">**ABAP + Java system**: **ASCS** instansen nummer **00** och **SCS** instansen nummer **01**</span><span class="sxs-lookup"><span data-stu-id="96390-959">**ABAP+Java system**: **ASCS** instance number **00** and **SCS** instance number **01**</span></span>

  <span data-ttu-id="96390-960">Om du vill använda instans siffror än 00 för ABAP ASCS-instansen och 01 för Java SCS-instans måste du ändra den Azure interna standard belastningsutjämnare NLB regler som beskrivs i [ändra ASCS/SCS standard belastningsutjämning regler för den interna belastningsutjämnaren som Azure][sap-ha-guide-8.9].</span><span class="sxs-lookup"><span data-stu-id="96390-960">To use instance numbers other than 00 for the ABAP ASCS instance and 01 for the Java SCS instance, first you need to change the Azure internal load balancer default load balancing rules, described in [Change the ASCS/SCS default load balancing rules for the Azure internal load balancer][sap-ha-guide-8.9].</span></span>

<span data-ttu-id="96390-961">Nästa några åtgärder beskrivs inte i dokumentationen för standard SAP-installationen.</span><span class="sxs-lookup"><span data-stu-id="96390-961">The next few tasks aren't described in the standard SAP installation documentation.</span></span>

> [!NOTE]
> <span data-ttu-id="96390-962">I dokumentationen för installation av SAP beskriver hur du installerar den första noden i klustret ASCS/SCS.</span><span class="sxs-lookup"><span data-stu-id="96390-962">The SAP installation documentation describes how to install the first ASCS/SCS cluster node.</span></span>
>
>

#### <span data-ttu-id="96390-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Ändra SAP-profil för ASCS/SCS-instansen</span><span class="sxs-lookup"><span data-stu-id="96390-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Modify the SAP profile of the ASCS/SCS instance</span></span>

<span data-ttu-id="96390-964">Du måste lägga till en ny profil-parameter.</span><span class="sxs-lookup"><span data-stu-id="96390-964">You need to add a new profile parameter.</span></span> <span data-ttu-id="96390-965">Parametern profil förhindrar anslutningar mellan SAP arbetsprocesser och placera servern stängs när de är inaktiv för länge.</span><span class="sxs-lookup"><span data-stu-id="96390-965">The profile parameter prevents connections between SAP work processes and the enqueue server from closing when they are idle for too long.</span></span> <span data-ttu-id="96390-966">Vi har nämnt problemet i [lägga till registerposter på båda klusternoderna för SAP ASCS/SCS-instansen][sap-ha-guide-8.11].</span><span class="sxs-lookup"><span data-stu-id="96390-966">We mentioned the problem scenario in [Add registry entries on both cluster nodes of the SAP ASCS/SCS instance][sap-ha-guide-8.11].</span></span> <span data-ttu-id="96390-967">I det här avsnittet introduceras vi några grundläggande TCP/IP-anslutningsparametrar också två ändringar.</span><span class="sxs-lookup"><span data-stu-id="96390-967">In that section, we also introduced two changes to some basic TCP/IP connection parameters.</span></span> <span data-ttu-id="96390-968">I nästa steg behöver du sätta servern ska skicka en `keep_alive` signalerar så att anslutningarna inte nått Azure interna belastningsutjämnarens inaktiv tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="96390-968">In a second step, you need to set the enqueue server to send a `keep_alive` signal so that the connections don't hit the Azure internal load balancer's idle threshold.</span></span>

<span data-ttu-id="96390-969">Ändra SAP-profilen för ASCS/SCS-instans:</span><span class="sxs-lookup"><span data-stu-id="96390-969">To modify the SAP profile of the ASCS/SCS instance:</span></span>

1.  <span data-ttu-id="96390-970">Lägg till den här profilen parametern SAP ASCS/SCS instans-profilen:</span><span class="sxs-lookup"><span data-stu-id="96390-970">Add this profile parameter to the SAP ASCS/SCS instance profile:</span></span>

  ```
  enque/encni/set_so_keepalive = true
  ```
  <span data-ttu-id="96390-971">I vårt exempel är sökvägen:</span><span class="sxs-lookup"><span data-stu-id="96390-971">In our example, the path is:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  <span data-ttu-id="96390-972">Till exempel till SCS SAP instans profil och motsvarande sökväg:</span><span class="sxs-lookup"><span data-stu-id="96390-972">For example, to the SAP SCS instance profile and corresponding path:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  <span data-ttu-id="96390-973">Om du vill tillämpa ändringarna SAP ASCS /SCS-instansen startas om.</span><span class="sxs-lookup"><span data-stu-id="96390-973">To apply the changes, restart the SAP ASCS /SCS instance.</span></span>

#### <span data-ttu-id="96390-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Lägga till en avsökningsport</span><span class="sxs-lookup"><span data-stu-id="96390-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Add a probe port</span></span>

<span data-ttu-id="96390-975">Använd den interna belastningsutjämnaren avsökningen funktioner för att göra hela klusterkonfigurationen arbeta med Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="96390-975">Use the internal load balancer's probe functionality to make the entire cluster configuration work with Azure Load Balancer.</span></span> <span data-ttu-id="96390-976">Azure intern belastningsutjämnare distribuerar vanligtvis inkommande belastningen jämnt mellan deltagande virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96390-976">The Azure internal load balancer usually distributes the incoming workload equally between participating virtual machines.</span></span> <span data-ttu-id="96390-977">Men fungerar det inte i vissa klusterkonfigurationer eftersom endast en instans är aktiv.</span><span class="sxs-lookup"><span data-stu-id="96390-977">However, this won't work in some cluster configurations because only one instance is active.</span></span> <span data-ttu-id="96390-978">Den andra instansen är passiva och kan inte ta emot någon av arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="96390-978">The other instance is passive and can’t accept any of the workload.</span></span> <span data-ttu-id="96390-979">Det hjälper dig att en avsökning funktionalitet när Azure intern belastningsutjämnare tilldelar fungerar endast för en aktiv instans.</span><span class="sxs-lookup"><span data-stu-id="96390-979">A probe functionality helps when the Azure internal load balancer assigns work only to an active instance.</span></span> <span data-ttu-id="96390-980">Med funktionen avsökningen identifiera den interna belastningsutjämnaren vilka instanser som är aktiva och rikta endast instansen med arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="96390-980">With the probe functionality, the internal load balancer can detect which instances are active, and then target only the instance with the workload.</span></span>

<span data-ttu-id="96390-981">Lägg till en avsökningsport:</span><span class="sxs-lookup"><span data-stu-id="96390-981">To add a probe port:</span></span>

1.  <span data-ttu-id="96390-982">Kontrollera aktuellt **ProbePort** inställningen genom att köra följande PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="96390-982">Check the current **ProbePort** setting by running the following PowerShell command.</span></span> <span data-ttu-id="96390-983">Köra den från inom någon av de virtuella datorerna i klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="96390-983">Execute it from within one of the virtual machines in the cluster configuration.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  <span data-ttu-id="96390-984">Definiera en avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="96390-984">Define a probe port.</span></span> <span data-ttu-id="96390-985">Avsökningen Standardportnumret är **0**.</span><span class="sxs-lookup"><span data-stu-id="96390-985">The default probe port number is **0**.</span></span> <span data-ttu-id="96390-986">I vårt exempel vi använder avsökningsport **62000**.</span><span class="sxs-lookup"><span data-stu-id="96390-986">In our example, we use probe port **62000**.</span></span>

  ![Bild 58: Avsökningsport för kluster-konfigurationen är 0 som standard][sap-ha-guide-figure-3048]

  <span data-ttu-id="96390-988">_**Bild 58:** klustret configuration avsökningen standardporten är 0_</span><span class="sxs-lookup"><span data-stu-id="96390-988">_**Figure 58:** The default cluster configuration probe port is 0_</span></span>

  <span data-ttu-id="96390-989">Portnumret är definierad i SAP Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="96390-989">The port number is defined in SAP Azure Resource Manager templates.</span></span> <span data-ttu-id="96390-990">Du kan tilldela portnummer i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96390-990">You can assign the port number in PowerShell.</span></span>

  <span data-ttu-id="96390-991">Ange ett nytt ProbePort värde för den  **SAP <*SID*> IP ** klusterresurs, kör följande PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="96390-991">To set a new ProbePort value for the **SAP <*SID*> IP** cluster resource, run the following PowerShell script.</span></span> <span data-ttu-id="96390-992">Uppdatera variablerna PowerShell för din miljö.</span><span class="sxs-lookup"><span data-stu-id="96390-992">Update the PowerShell variables for your environment.</span></span> <span data-ttu-id="96390-993">När skriptet körs, uppmanas du att starta om SAP klustergrupp för att aktivera ändringarna.</span><span class="sxs-lookup"><span data-stu-id="96390-993">After the script runs, you'll be prompted to restart the SAP cluster group to activate the changes.</span></span>

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  Clear-Host
  $SAPClusterRoleName = "SAP $SAPSID"
  $SAPIPresourceName = "SAP $SAPSID IP"
  $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
  $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
  $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
  $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
  $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
  $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
  $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

  $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

  Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

  Write-Host
  Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

  if($ActivateChanges -eq "yes"){
  Write-Host
  Write-Host "Activating changes..." -ForegroundColor Cyan

  Write-Host
  write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
  Stop-ClusterResource -Name $SAPIPresourceName
  sleep 5

  Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
  Start-ClusterGroup -Name $SAPClusterRoleName

  Write-Host "New ProbePort parameter is active." -ForegroundColor Green
  Write-Host

  Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
  Write-Host
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
  }else
  {
  Write-Host "Changes are not activated."
  }
  ```

  <span data-ttu-id="96390-994">När du tar den  **SAP <*SID*> ** rollen online bör du kontrollera att **ProbePort** är inställd på det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="96390-994">After you bring the **SAP <*SID*>** cluster role online, verify that **ProbePort** is set to the new value.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Bild 59: Avsökning porten klustret när du ställer in det nya värdet][sap-ha-guide-figure-3049]

  <span data-ttu-id="96390-996">_**Bild 59:** avsökning porten klustret när du ställer in det nya värdet_</span><span class="sxs-lookup"><span data-stu-id="96390-996">_**Figure 59:** Probe the cluster port after you set the new value_</span></span>

#### <span data-ttu-id="96390-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Öppna avsökningsport för Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="96390-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open the Windows firewall probe port</span></span>

<span data-ttu-id="96390-998">Du måste öppna en Windows-brandväggen avsökningsport på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="96390-998">You need to open a Windows firewall probe port on both cluster nodes.</span></span> <span data-ttu-id="96390-999">Använd följande skript för att öppna en Windows-brandväggen avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="96390-999">Use the following script to open a Windows firewall probe port.</span></span> <span data-ttu-id="96390-1000">Uppdatera variablerna PowerShell för din miljö.</span><span class="sxs-lookup"><span data-stu-id="96390-1000">Update the PowerShell variables for your environment.</span></span>

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

<span data-ttu-id="96390-1001">Den **ProbePort** är inställd på **62000**.</span><span class="sxs-lookup"><span data-stu-id="96390-1001">The **ProbePort** is set to **62000**.</span></span> <span data-ttu-id="96390-1002">Nu kan du komma åt filresursen  **\\\ascsha-clsap\sapmnt** från andra värdar, exempel från **ascsha dbas**.</span><span class="sxs-lookup"><span data-stu-id="96390-1002">Now you can access the file share **\\\ascsha-clsap\sapmnt** from other hosts, such as from **ascsha-dbas**.</span></span>

### <span data-ttu-id="96390-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Installera databasinstansen</span><span class="sxs-lookup"><span data-stu-id="96390-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Install the database instance</span></span>

<span data-ttu-id="96390-1004">För att installera databasinstansen, följer du processen som beskrivs i dokumentationen för SAP-installationen.</span><span class="sxs-lookup"><span data-stu-id="96390-1004">To install the database instance, follow the process described in the SAP installation documentation.</span></span>

### <span data-ttu-id="96390-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Installera den andra noden i klustret</span><span class="sxs-lookup"><span data-stu-id="96390-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Install the second cluster node</span></span>

<span data-ttu-id="96390-1006">Följ stegen i installationsguiden för SAP för att installera det andra klustret.</span><span class="sxs-lookup"><span data-stu-id="96390-1006">To install the second cluster, follow the steps in the SAP installation guide.</span></span>

### <span data-ttu-id="96390-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Ändra starttypen för SAP ÄNDARE Windows service-instans</span><span class="sxs-lookup"><span data-stu-id="96390-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Change the start type of the SAP ERS Windows service instance</span></span>

<span data-ttu-id="96390-1008">Ändra starttypen för tjänsten SAP ÄNDARE Windows att **automatiskt (förskjuten Start)** på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="96390-1008">Change the start type of the SAP ERS Windows service to **Automatic (Delayed Start)** on both cluster nodes.</span></span>

![Bild 60: Ändra service-typen för instansen SAP ÄNDARE till fördröjd automatisk][sap-ha-guide-figure-3050]

<span data-ttu-id="96390-1010">_**Bild 60:** ändra tjänsttypen för SAP ÄNDARE-instans till fördröjd automatisk_</span><span class="sxs-lookup"><span data-stu-id="96390-1010">_**Figure 60:** Change the service type for the SAP ERS instance to delayed automatic_</span></span>

### <span data-ttu-id="96390-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Installera den primära SAP-programservern</span><span class="sxs-lookup"><span data-stu-id="96390-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Install the SAP Primary Application Server</span></span>

<span data-ttu-id="96390-1012">Installera den primära programmet Server (PROVIDERADRESSER)-instansen <*SID*> - di - 0 på den virtuella datorn som du har angett som värd för PROVIDERADRESSERNA.</span><span class="sxs-lookup"><span data-stu-id="96390-1012">Install the Primary Application Server (PAS) instance <*SID*>-di-0 on the virtual machine that you've designated to host the PAS.</span></span> <span data-ttu-id="96390-1013">Det finns inga beroenden på Azure eller DataKeeper-specifika inställningar.</span><span class="sxs-lookup"><span data-stu-id="96390-1013">There are no dependencies on Azure or DataKeeper-specific settings.</span></span>

### <span data-ttu-id="96390-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Installera ytterligare programservern för SAP</span><span class="sxs-lookup"><span data-stu-id="96390-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Install the SAP Additional Application Server</span></span>

<span data-ttu-id="96390-1015">Installera en SAP ytterligare program Server (AAS) på alla virtuella datorer som du har angett som värd för en SAP Application Server-instans.</span><span class="sxs-lookup"><span data-stu-id="96390-1015">Install an SAP Additional Application Server (AAS) on all the virtual machines that you've designated to host an SAP Application Server instance.</span></span> <span data-ttu-id="96390-1016">Till exempel på <*SID*> - di - 1 till <*SID*> - di -&lt;n&gt;.</span><span class="sxs-lookup"><span data-stu-id="96390-1016">For example, on <*SID*>-di-1 to <*SID*>-di-&lt;n&gt;.</span></span>

> [!NOTE]
> <span data-ttu-id="96390-1017">Installationen av ett system med hög tillgänglighet SAP NetWeaver slutförs.</span><span class="sxs-lookup"><span data-stu-id="96390-1017">This finishes the installation of a high-availability SAP NetWeaver system.</span></span> <span data-ttu-id="96390-1018">Fortsätt sedan med testning av redundans.</span><span class="sxs-lookup"><span data-stu-id="96390-1018">Next, proceed with failover testing.</span></span>
>


## <span data-ttu-id="96390-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testa redundans för SAP ASCS/SCS-instans och SIOS replikering</span><span class="sxs-lookup"><span data-stu-id="96390-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Test the SAP ASCS/SCS instance failover and SIOS replication</span></span>
<span data-ttu-id="96390-1020">Det är enkelt att testa och övervaka en växling vid fel för SAP ASCS/SCS-instans och SIOS diskreplikering med hjälp av verktyg för hanteraren för redundanskluster och SIOS DataKeeper hantering och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="96390-1020">It's easy to test and monitor an SAP ASCS/SCS instance failover and SIOS disk replication by using Failover Cluster Manager and the SIOS DataKeeper Management and Configuration tool.</span></span>

### <span data-ttu-id="96390-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASCS/SCS-instans som körs på en nod i klustret</span><span class="sxs-lookup"><span data-stu-id="96390-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS instance is running on cluster node A</span></span>

<span data-ttu-id="96390-1022">Den **SAP PR1** klustergrupp körs på klusternoden A. Till exempel på **pr1-ascs-0**.</span><span class="sxs-lookup"><span data-stu-id="96390-1022">The **SAP PR1** cluster group is running on cluster node A. For example, on **pr1-ascs-0**.</span></span> <span data-ttu-id="96390-1023">Tilldela den delade diskenheten S, som är en del av den **SAP PR1** klustergrupp och som använder ASCS/SCS-instans för klustret nod A.</span><span class="sxs-lookup"><span data-stu-id="96390-1023">Assign the shared disk drive S, which is part of the **SAP PR1** cluster group, and which the ASCS/SCS instance uses, to cluster node A.</span></span>

![Bild 61: Hanteraren för redundanskluster: det SAP < SID > klustergrupp körs på en nod i klustret][sap-ha-guide-figure-5000]

<span data-ttu-id="96390-1025">_**Bild 61:** Klusterhanterare för växling vid fel: det SAP <*SID*> klustergrupp körs på en nod i klustret_</span><span class="sxs-lookup"><span data-stu-id="96390-1025">_**Figure 61:** Failover Cluster Manager: The SAP <*SID*> cluster group is running on cluster node A_</span></span>

<span data-ttu-id="96390-1026">I verktyget SIOS DataKeeper hantering och konfiguration kan du se att delad diskdata replikeras synkront i från volymen källenheten S på en nod i klustret till volymen målenheten S på klusternoden B. Till exempel har replikerats från **pr1-ascs-0 [10.0.0.40]** till **pr1-ascs-1 [10.0.0.41]**.</span><span class="sxs-lookup"><span data-stu-id="96390-1026">In the SIOS DataKeeper Management and Configuration tool, you can see that the shared disk data is synchronously replicated from the source volume drive S on cluster node A to the target volume drive S on cluster node B. For example, it's replicated from **pr1-ascs-0 [10.0.0.40]** to **pr1-ascs-1 [10.0.0.41]**.</span></span>

![Bild 62: I SIOS DataKeeper replikera lokala volymen från en nod i klustret till klusternod B][sap-ha-guide-figure-5001]

<span data-ttu-id="96390-1028">_**Bild 62:** i SIOS DataKeeper replikera lokala volymen från en nod i klustret till klusternoden B_</span><span class="sxs-lookup"><span data-stu-id="96390-1028">_**Figure 62:** In SIOS DataKeeper, replicate the local volume from cluster node A to cluster node B_</span></span>

### <span data-ttu-id="96390-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Växling vid fel från nod A till B-nod</span><span class="sxs-lookup"><span data-stu-id="96390-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover from node A to node B</span></span>

1.  <span data-ttu-id="96390-1030">Välj något av dessa alternativ för att påbörja en växling av SAP <*SID*> klustergrupp från klusternoden A klusternoden B:</span><span class="sxs-lookup"><span data-stu-id="96390-1030">Choose one of these options to initiate a failover of the SAP <*SID*> cluster group from cluster node A to cluster node B:</span></span>
  - <span data-ttu-id="96390-1031">Använd hanteraren för redundanskluster</span><span class="sxs-lookup"><span data-stu-id="96390-1031">Use Failover Cluster Manager</span></span>  
  - <span data-ttu-id="96390-1032">Använda PowerShell för Failover-kluster</span><span class="sxs-lookup"><span data-stu-id="96390-1032">Use Failover Cluster PowerShell</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  <span data-ttu-id="96390-1033">Starta om klustret nod A Windows-gästoperativsystem (Detta startar en automatisk redundans för SAP <*SID*> klustergrupp från nod A till B-nod).</span><span class="sxs-lookup"><span data-stu-id="96390-1033">Restart cluster node A within the Windows guest operating system (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
3.  <span data-ttu-id="96390-1034">Starta om klustret nod A från Azure-portalen (Detta startar en automatisk redundans för SAP <*SID*> klustergrupp från nod A till B-nod).</span><span class="sxs-lookup"><span data-stu-id="96390-1034">Restart cluster node A from the Azure portal (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
4.  <span data-ttu-id="96390-1035">Starta om klustret nod A med hjälp av Azure PowerShell (Detta startar en automatisk redundans för SAP <*SID*> klustergrupp från nod A till B-nod).</span><span class="sxs-lookup"><span data-stu-id="96390-1035">Restart cluster node A by using Azure PowerShell (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>

  <span data-ttu-id="96390-1036">Efter växling vid fel, SAP <*SID*> klustergrupp körs på klusternoden B. Till exempel den körs på **pr1-ascs-1**.</span><span class="sxs-lookup"><span data-stu-id="96390-1036">After failover, the SAP <*SID*> cluster group is running on cluster node B. For example, it's running on **pr1-ascs-1**.</span></span>

  ![Bild 63: I Hanteraren för redundanskluster, SAP < SID > klustergrupp körs på klusternoden B][sap-ha-guide-figure-5002]

  <span data-ttu-id="96390-1038">_**Bild 63**: I Hanteraren för redundanskluster, SAP <*SID*> klustergrupp körs på klusternoden B_</span><span class="sxs-lookup"><span data-stu-id="96390-1038">_**Figure 63**: In Failover Cluster Manager, the SAP <*SID*> cluster group is running on cluster node B_</span></span>

  <span data-ttu-id="96390-1039">Den delade disken nu är monterad på klustret nod B. SIOS DataKeeper replikerar data från volymen källenheten S på klusternoden B till volymen målenheten S på klusternoden A. Till exempel den replikerar från **pr1-ascs-1 [10.0.0.41]** till **pr1-ascs-0 [10.0.0.40]**.</span><span class="sxs-lookup"><span data-stu-id="96390-1039">The shared disk is now mounted on cluster node B. SIOS DataKeeper is replicating data from source volume drive S on cluster node B to target volume drive S on cluster node A. For example, it's replicating from **pr1-ascs-1 [10.0.0.41]** to **pr1-ascs-0 [10.0.0.40]**.</span></span>

  ![Bild 64: SIOS DataKeeper replikerar den lokala volymen från klusternoden B klustra nod A][sap-ha-guide-figure-5003]

  <span data-ttu-id="96390-1041">_**Bild 64:** SIOS DataKeeper replikerar den lokala volymen från klusternoden B klustra nod A_</span><span class="sxs-lookup"><span data-stu-id="96390-1041">_**Figure 64:** SIOS DataKeeper replicates the local volume from cluster node B to cluster node A_</span></span>
