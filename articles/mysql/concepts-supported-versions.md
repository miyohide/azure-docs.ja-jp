---
title: Azure Database for MySQL のサポートされているバージョン
description: サポートされている Azure Database for MySQL バージョンについて説明します。
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: f2a9348e267ad6a5929fda64cc08dcd837243e71
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093342"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>サポートされている Azure Database for MySQL サーバー バージョン
Azure Database for MySQL は、InnoDB エンジンを使用して [MySQL Community Edition](https://www.mysql.com/products/community/) から開発されました。 Azure Database for MySQL では現在、次のバージョンがサポートされています。

## <a name="mysql-version-5639"></a>MySQL バージョン 5.6.39
MySQL 5.6.39 の機能強化と修正については、MySQL の[ドキュメント](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html)を参照してください。

## <a name="mysql-version-5721"></a>MySQL バージョン 5.7.21
MySQL 5.7.21 の機能強化と修正については、MySQL の[ドキュメント](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html)を参照してください。

> [!NOTE]
> サービスでは、接続をサーバー インスタンスにリダイレクトするためにゲートウェイが使用されます。 接続が確立すると、MySQL には、MySQL サーバー インスタンスで実行されている実際のバージョンではなく、ゲートウェイに設定されている MySQL のバージョンが表示されます。 MySQL サーバー インスタンスのバージョンを判断するには、MySQL プロンプトで `SELECT VERSION();` コマンドを使用します。

## <a name="managing-updates-and-upgrades"></a>更新プログラムとアップグレードの管理
このサービスでは、バグ修正バージョンの更新プログラムの適用が自動管理されます。 現在、マイナー バージョンのアップグレードはサポートされていません。 たとえば、MySQL 5.6 から MySQL 5.7 へのアップグレードはサポートされません。 次のマイナー バージョンにアップグレードする場合は、新しいエンジンのバージョンで作成されたサーバーに[ダンプを復元](./concepts-migrate-dump-restore.md)します。

## <a name="next-steps"></a>次の手順

**サービス レベル**に基づく特定のリソース クォータと制限については、[サービス レベル](./concepts-pricing-tiers.md)に関するページをご覧ください
