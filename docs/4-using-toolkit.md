[←Microsoft Graphを利用するWebアプリケーションを作る](./2-make-web-app.md)

では、Graph Tool Kitを使ってログイン画面とユーザー情報の表示を実装してみます。

``` html
<html>
  <head>
    <script src="https://unpkg.com/@microsoft/mgt/dist/bundle/mgt-loader.js"></script>
  </head>
  <body>
    <mgt-msal2-provider client-id="<ClientId>"
      authority="https://login.microsoftonline.com/<TenantId>/"></mgt-msal2-provider>
    <mgt-login></mgt-login>
    <mgt-agenda></mgt-agenda>
  </body>
</html>
```

これで実装完了です。

先程自分で実装した画面同様、ログインボタン押下後、ログインすることで、画面にログインしているユーザーと予定表の情報が表示されます。

## コードの解説

### script内

`<script>`タグで読み込んでいるのはGraph ToolKitのライブラリです。

これを読み込むことで、以降の`mgt-xxx`タグが使用できるようになります。

タグは[Web Components](https://developer.mozilla.org/ja/docs/Web/Web_Components)と呼ばれるWeb標準の構成で実装されており、そのおかげで`div`などのタグと同じ感じでHTMLテンプレート上で使用できるようになります。

タグで提供される1機能郡はComponentと呼ばれる単位で、内部機能やレイアウトを提供しています。

そのため利用するHTML上で多くの処理を書かなくて良い構成が実現できています。

### mgt-msal2-provider

自前で実装したアプリケーションの`msal`ライブラリの機能を有しています。

GraphToolKitはその名前の通り、Microsoft Graphにアクセスしデータを取得します。

そのため認証を必要とするのですが、その認証機能をプロバイドしてくれるのがこのタグです。

msal2以外に下記のプロバイダが存在します。アプリケーションの展開先によって使用するプロバイダを選択してあげればいいと思います。

* msalプロバイダ(`mgt-msal-provider`)
* SharePointプロバイダ(JSで使用)
* Teamsプロバイダ(JSで使用)
* Teams msal2プロバイダ(JSで使用)
* Electronプロバイダ(JSで使用)

### mgt-login

ログイン機能を提供するComponentです。

この中に先で実装したログインボタンの中の制御やユーザー情報の取得

アイコン情報の取得、ボタン押下後の制御なんかがすべて実装されています。

Componentでそれらの処理がパッケージングされて提供されているので、このタグだけでそれらの機能を利用することが可能です。

### mgt-agenda

予定の出力機能を提供するComponentです。

この中に先で実装した予定の取得や表示の制御なんかがすべて実装されています。


[mg-getで提供されていないリソースを利用する→](./4-custom-component.md)

# 関連ドキュメント

* [Microsoft Graph Toolkit](https://docs.microsoft.com/ja-jp/graph/toolkit/get-started/overview?tabs=html)
* [Microsoft Graph Toolkit の ログイン コンポーネント](https://docs.microsoft.com/ja-jp/graph/toolkit/components/login?context=graph%2Fapi%2F1.0&view=graph-rest-1.0)



