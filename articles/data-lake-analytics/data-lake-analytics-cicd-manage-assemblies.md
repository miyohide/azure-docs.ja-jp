---
title: Azure Data Lake の CI/CD パイプラインで U-SQL アセンブリを管理するためのベスト プラクティス
description: Azure DevOps を使用して CI/CD パイプラインで U-SQL C# アセンブリを管理するためのベスト プラクティスについて学習します。
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0f6f581d90b025dd538eb9b79da1a0addd4259f7
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670289"
---
# <a name="best-practice-of-managing-u-sql-assemblies-in-cicd-pipeline"></a>CI/CD パイプラインで U-SQL アセンブリを管理するためのベスト プラクティス

この記事では、新しく導入された U-SQL データベース プロジェクトを使用して、U-SQL アセンブリのソース コードを管理する方法について説明します。 また、Azure DevOps を使用して、アセンブリ登録のための継続的統合およびデプロイ (CI/CD) パイプラインを設定する方法について説明します。

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>U-SQL データベース プロジェクトを使用してアセンブリのソース コードを管理する

[U-SQL データベース プロジェクト](data-lake-analytics-data-lake-tools-develop-usql-database.md)は、開発者が高速かつ簡単に U-SQL データベースの開発、管理、および配置を行えるようにする Visual Studio のプロジェクトの一種です。 すべての U-SQL データベース オブジェクト (資格情報を除く) は、U-SQL データベース プロジェクトで管理できます。 

C# アセンブリ ソース コードとアセンブリ登録 DDL U-SQL スクリプトを管理するには、以下の方法が推奨されます。

* **U-SQL データベース プロジェクト**を使用して、アセンブリ登録 U-SQL スクリプトを管理します。
* **クラス ライブラリ (U-SQL アプリケーション用)** を使用して、C# のソースコードおよびユーザー定義の演算子、関数、およびアグリゲーター (UDO/UDF/UDAG) の依存関係を管理します。
* U-SQL データベース プロジェクトを使用して、クラス ライブラリ プロジェクトを参照します。 

U-SQL データベース プロジェクトは、クラス ライブラリ (U-SQL アプリケーション用の) プロジェクトを参照できます。 U-SQL データベースに登録されているアセンブリは、参照される C# ソース コードを使用して、このクラス ライブラリ (U-SQL アプリケーション用の) プロジェクトから作成できます。

次の手順に従って、プロジェクトを作成して参照を追加することができます。
1. **[File]\(ファイル\) > [New]\(新規\) > [Project]\(プロジェクト\)** の順に選択して、[Class Library (For U-SQL Application)] プロジェクトを作成します。 プロジェクトは **[Azure Data Lake] > [U-SQL]** ノードにあります。
   ![Data Lake Tools for Visual Studio -- C# クラス ライブラリ プロジェクトの作成](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
2. ユーザー定義の C# コードを [Class Library (For U-SQL Application)] プロジェクトに追加します。 
3. **[File]\(ファイル\) > [New]\(新規\) > [Project]\(プロジェクト\)** の順に選択して、U-SQL プロジェクトを作成します。 プロジェクトは **[Azure Data Lake] > [U-SQL]** ノードにあります。
   ![Data Lake Tools for Visual Studio -- U-SQL データベース プロジェクトの作成](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
4. U-SQL データベース プロジェクトのために、C# クラス ライブラリ プロジェクトの参照を追加します。

    ![Data Lake Tools for Visual Studio -- U-SQL データベース プロジェクト参照の追加](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools for Visual Studio -- U-SQL データベース プロジェクト参照の追加](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)
5. **プロジェクトを右クリック > [Add New Item]\(新しい項目の追加)** を選択して、U-SQL データベース プロジェクトにアセンブリ スクリプトを作成します。
   ![Data Lake Tools for Visual Studio -- アセンブリ スクリプトの追加](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)
6. アセンブリ スクリプトを開き、アセンブリ デザイン ビューで、**[Create assembly from reference]\(参照からアセンブリを作成\)** ボックスの一覧から参照先アセンブリを選択します。

    ![Data Lake Tools for Visual Studio -- 参照からのアセンブリの作成](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. **[マネージドの依存関係]** と **[追加ファイル]** が存在する場合は追加します。 追加ファイルを追加する場合、ローカル コンピューターと後のビルド コンピューターの両方で確実にアセンブリを検索できるようにするため、ツールは相対パスを使用します。 

下部にあるエディター ウィンドウの **@_DeployTempDirectory** は、ビルド出力フォルダーを指すツールの定義済み変数です。 ビルド出力フォルダーには、アセンブリ名を名前とするサブフォルダーがすべてのアセンブリにあり、 すべての DLL および追加ファイルがそのサブフォルダーに格納されます。 

## <a name="build-a-u-sql-database-project"></a>U-SQL データベース プロジェクトの作成

U-SQL データベース プロジェクトのビルド出力は、名前に `.usqldbpack` というサフィックスのついた U-SQL データベース展開パッケージです。 `.usqldbpack` パッケージは .zip ファイルで、**DDL** フォルダー内の単一の U-SQL スクリプトにはすべての DDL ステートメントが、**Temp** フォルダーにはアセンブリのすべてのビルド済み DLL と追加ファイルが含まれています。

## <a name="deploy-a-u-sql-database"></a>U-SQL データベースの配置

`.usqldbpack` パッケージは、ローカル アカウントと Azure Data Lake Analytics アカウントのどちらにでも、Visual Studio またはデプロイ SDK を使用して配置できます。 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Visual Studio での U-SQL データベースの配置

Visual Studio で、U-SQL データベース プロジェクトまたは `.usqldbpack` パッケージを使用して U-SQL データベースを配置できます。

#### <a name="deploy-through-a-u-sql-database-project"></a>U-SQL データベース プロジェクトによる配置

1.  U-SQL データベース プロジェクトを右クリックし、**[展開]** を選択します。
2.  **U-SQL データベースの配置**ウィザードで、データベースを配置する **[ADLA account]\(ADLA アカウント\)** を選択します。 ローカル アカウントと ADLA アカウントの両方がサポートされています。
3.  **[Database Source]\(データベース ソース\)** は、プロジェクトのビルド出力フォルダー内の .usqldbpack パッケージを指すように自動的に設定されます。
4.  データベースを作成するには、**[データベース名]** に名前を入力します。 ターゲットの Azure Data Lake Analytics アカウントに同じ名前のデータベースが存在する場合、データベースは再作成されず、データベース プロジェクトに定義されているすべてのオブジェクトが作成されます。
5.  U-SQL データベースを配置するには、**[送信]** を選択します。 すべてのリソース (アセンブリおよび追加ファイル) がアップロードされ、すべての DDL ステートメントを含む U-SQL ジョブが送信されます。

    ![Data Lake Tools for Visual Studio -- U-SQL データベース プロジェクトの配置](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools for Visual Studio -- U-SQL データベース プロジェクトの配置ウィザード](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-u-sql-database-in-azure-devops"></a>Azure DevOps での U-SQL データベースの配置

`PackageDeploymentTool.exe` は、U-SQL データベースを配置する際に役立つプログラミングとコマンド ライン インターフェイスを提供します。 この SDK は [U-SQL SDK Nuget パッケージ](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)に含まれており、`build/runtime/PackageDeploymentTool.exe` の場所にあります。

Azure DevOps では、コマンド ライン タスクとこの SDK を使用して、U-SQL データベース更新のためのオートメーション パイプラインを設定できます。 [SDK と、U-SQL データベースを配置する CI/CD パイプラインの設定方法の詳細については、こちらを参照してください](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines)。

## <a name="next-steps"></a>次の手順

* [Azure Data Lake Analytics の CI/CD パイプラインをセットアップする方法](data-lake-analytics-cicd-overview.md)
* [Azure Data Lake Analytics コードをテストする方法](data-lake-analytics-cicd-test.md)
* [ローカル コンピューターで U-SQL スクリプトを実行する](data-lake-analytics-data-lake-tools-local-run.md)
