---
title: Azure でデータ サイエンス環境を設定する | Microsoft Docs
description: Team Data Science Process で使うためのデータ サイエンス環境を Azure で設定します。
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 481cfa6a-7ea3-46ac-b0f9-2e3982c37153
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: deguhath
ms.openlocfilehash: 996c7b4332326ddf1ef18ca732677a85b6df6046
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345982"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Team Data Science Process で使用するためのデータ サイエンス環境の設定
Team Data Science Process は、データの格納、処理、および分析のために、さまざまなデータ サイエンス環境を使用します。 たとえば、Azure Blob Storage、いくつかの種類の Azure 仮想マシン、HDInsight (Hadoop) クラスター、Azure Machine Learning ワークスペースなどです。 使用する環境を決定する方法は、モデル化するデータの種類と量、さらにクラウド内でのデータの宛先によって異なります。 

* この決定を行う際の考慮事項については、「[Azure Machine Learning のデータ サイエンス環境を計画する](plan-your-environment.md)」をご覧ください。 
* 高度な分析を実施する際に発生する可能性があるシナリオのカタログについては、 [Team Data Science Process のシナリオ](plan-sample-scenarios.md)

以下の記事では、Team Data Science Process によって使用されるさまざまなデータ サイエンス環境の設定方法が説明されています。

* [Azure のストレージ アカウント](../../storage/common/storage-quickstart-create-account.md)
* [HDInsight (Hadoop) クラスター](customize-hadoop-cluster.md)
* [Azure Machine Learning Studio ワークスペース](../studio/create-workspace.md)

**Microsoft データ サイエンス仮想マシン (DSVM)** は、Azure Virtual Machine (VM) イメージとしても利用できます。 VM は、あらかじめインストールされており、データ分析と機械学習用に一般的に使用されているいくつかのツールで構成されています。 DSVM は、Windows と Linux の両方で使用できます。 詳しくは、[Linux および Windows 用のクラウドベースの Data Science Virtual Machine の概要](../data-science-virtual-machine/overview.md)に関するページをご覧ください。

それぞれの作成方法に関するページを参照してください。

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [ディープ ラーニング VM](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
