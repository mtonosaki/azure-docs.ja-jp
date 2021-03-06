---
title: '複数の依存サービスの実行: Java と Visual Studio Code'
services: azure-dev-spaces
ms.date: 11/21/2018
ms.topic: tutorial
description: このチュートリアルでは、Azure Dev Spaces と Visual Studio Code を使用して、Azure Kubernetes Service 上でマルチサービス Java アプリケーションをデバッグする方法を示します。
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, コンテナー, Helm, サービス メッシュ, サービス メッシュのルーティング, kubectl, k8s
ms.openlocfilehash: beab91964cab9938a5d63584089326bb408f6efc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "75438327"
---
# <a name="running-multiple-dependent-services-java-and-visual-studio-code-with-azure-dev-spaces"></a>複数の依存サービスの実行: Azure Dev Spaces での Java と Visual Studio Code

このチュートリアルでは、Azure Dev Spaces を使用してマルチサービス アプリケーションを開発する方法と、Dev Spaces が提供するいくつかの追加の利点について学習します。

## <a name="call-a-service-running-in-a-separate-container"></a>別のコンテナーで実行されているサービスを呼び出す

このセクションでは、2 つ目のサービス (`mywebapi`) を作成し、`webfrontend` でそのサービスを呼び出します。 各サービスは別々のコンテナーで実行されます。 その後、両方のコンテナーでデバッグします。

![複数のコンテナー](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>*mywebapi* のサンプル コードをダウンロードする
時間を節約するために、GitHub リポジトリからサンプル コードをダウンロードしましょう。 [https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces ) に移動し、 **[Clone or download]** をクリックして GitHub リポジトリをダウンロードします。 このセクションのコードは、`samples/java/getting-started/mywebapi` にあります。

### <a name="run-mywebapi"></a>*mywebapi* を実行する
1. "*別の VS Code ウィンドウ*" で、`mywebapi` フォルダーを開きます。
1. ( **[表示 | コマンド パレット]** メニューを使用して) **コマンド パレット**を開き、オート コンプリートを使用して次のコマンドを入力および選択します: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`。
1. F5 キーを押し、サービスがビルドされ、展開されるまで待ちます。 準備ができると、次のようなメッセージがデバッグ コンソールに表示されます。

    ```cmd
    2019-03-11 17:02:35.935  INFO 216 --- [           main] com.ms.sample.mywebapi.Application       : Started Application in 8.164 seconds (JVM running for 9.272)
    ```

1. エンドポイント URL は `http://localhost:<portnumber>` のように表記されます。 **ヒント: VS Code のステータス バーがオレンジ色に変わり、クリック可能な URL が表示されます。** コンテナーはローカルで実行されているように見えますが、実際には Azure の開発空間で実行されています。 localhost アドレスである理由は、`mywebapi` でパブリック エンドポイントが定義されておらず、Kubernetes インスタンス内からしかアクセスできないためです。 利便性を考慮し、また、ローカル コンピューターからのプライベート サービスとの対話を容易にするために、Azure Dev Spaces では、Azure で実行されているコンテナーへの一時的な SSH トンネルが作成されます。
1. `mywebapi` の準備ができたら、ブラウザーで localhost アドレスを開きます。
1. すべての手順が正常に完了すると、`mywebapi` サービスからの応答を確認できます。

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend* から *mywebapi* に対して要求を行う
次に、`mywebapi` に対して要求を行う `webfrontend` のコードを記述しましょう。
1. `webfrontend` の VS Code ウィンドウに切り替えます。
1. `package` ステートメントの下に、次の `import` ステートメントを*追加します*。

   ```java
   import java.io.*;
   import java.net.*;
   ```
1. 次のように、greeting メソッドのコードを*置き換えます*。

    ```java
    @RequestMapping(value = "/greeting", produces = "text/plain")
    public String greeting(@RequestHeader(value = "azds-route-as", required = false) String azdsRouteAs) throws Exception {
        URLConnection conn = new URL("http://mywebapi/").openConnection();
        conn.setRequestProperty("azds-route-as", azdsRouteAs); // propagate dev space routing header
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream())))
        {
            return "Hello from webfrontend and " + reader.lines().reduce("\n", String::concat);
        }
    }
    ```

前述のコード例は、`azds-route-as` ヘッダーを受信要求から送信要求に転送します。 これがチームによる共同開発にどのように役立つかについては、後ほど説明します。

### <a name="debug-across-multiple-services"></a>複数のサービスでデバッグする
1. この時点で、`mywebapi` は引き続きデバッガーがアタッチされた状態で実行されています。 そうでない場合は、`mywebapi` プロジェクトで F5 キーを押します。
1. `mywebapi` プロジェクトの `index()` メソッドでブレークポイントを設定します ([`Application.java` の 19 行目](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/mywebapi/src/main/java/com/ms/sample/mywebapi/Application.java#L19))。
1. `webfrontend` プロジェクトの `try` から始まる行で、GET 要求を `mywebapi` に送信する直前にブレークポイントを設定します。
1. `webfrontend` プロジェクトで F5 キーを押します (または現在実行中の場合はデバッガーを再起動します)。
1. Web アプリを起動し、両方のサービスでコードをステップ実行します。
1. Web アプリの [About]\(詳細情報\) ページに、2 つのサービスによって連結されたメッセージ "Hello from webfrontend and Hello from mywebapi" が表示されます。

### <a name="well-done"></a>お疲れさまでした。
これで、各コンテナーを個別に開発して展開できる、複数コンテナー アプリケーションが作成されました。


## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Dev Spaces でのチーム開発について学ぶ](team-development-java.md)
