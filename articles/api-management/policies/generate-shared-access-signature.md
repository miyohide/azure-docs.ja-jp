---
title: Azure API Management ポリシーのサンプル - Shared Access Signature の生成 | Microsoft Docs
description: Azure API Management ポリシーのサンプル - 式を使用して Shared Access Signature を生成し、rewrite-uri ポリシーを使用して要求を Azure ストレージに転送する方法を示します。
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: c8a4d25211a0030c013628e69865406bb6e8899e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286290"
---
# <a name="generate-shared-access-signature"></a>Shared Access Signature の生成

この記事では、式を使用して [Shared Access Signature](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) を生成し、rewrite-uri ポリシーを使用して Azure ストレージに要求を転送する方法を示す Azure API Management ポリシーのサンプルを示します。 ポリシー コードを設定または編集するには、「[ポリシーの設定または編集](../set-edit-policies.md)」で説明されている手順に従います。 他の例については、[ポリシーのサンプル](../policy-samples.md)に関するページをご覧ください。

## <a name="policy"></a>ポリシー

コードを **inbound** ブロックに貼り付けます。

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>次の手順

APIM ポリシーの詳細については、以下をご覧ください。

+ [変換ポリシー](../api-management-transformation-policies.md)
+ [ポリシーのサンプル](../policy-samples.md)

