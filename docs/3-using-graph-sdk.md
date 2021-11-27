[←Microsoft Graphを利用するWebアプリケーションを作る](./2-make-web-app.md)

Graphを利用する上で便利なツールとして`Graph SDK`が存在します。

index.htmlを置き換える形で実装してみます。

``` html
<html>
  <head>
    <script type="text/javascript" src="https://alcdn.msauth.net/browser/2.19.0/js/msal-browser.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@microsoft/microsoft-graph-client@3.0.0/lib/graph-js-sdk.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@microsoft/microsoft-graph-client@3.0.0/lib/graph-client-msalBrowserAuthProvider.js"></script>
    <style>
      #contents {
        height: 25px;
        line-height: 25px;
      }
      img {
        height: 25px;
        border-radius: 50%;
        vertical-align: middle;
      }
      .user-info {
        vertical-align: middle;
      }
    </style>
  </head>
  <div>
    <button id="login">ログイン</button>
  </div>
  <div id="contents"></div>
  <script>
    // 認証オブジェクトを作成
    const myMSALObj = new msal.PublicClientApplication({
      auth: {
        clientId: '<ClientId>',
        authority: 'https://login.microsoftonline.com/<TenantId>/'
      }
    });

    const btn = document.getElementById('login');

    const  loginAction = async () => {
      // ポップアップでログイン処理を実行
      const loginResult = await myMSALObj.loginPopup({ scopes: ['user.read'] });

      // 以降ログインが成功したときの処理
      // ログインボタンを消す
      btn.remove();

      const authProvider = new MSGraphAuthCodeMSALBrowserAuthProvider.AuthCodeMSALBrowserAuthenticationProvider(
      myMSALObj, {
        account: loginResult.account,
        scopes: ['user.read'],
        interactionType: msal.InteractionType.PopUp
      });
      const graphClient = MicrosoftGraph.Client.initWithMiddleware({authProvider});

      // ユーザー情報をMicrosoft Graph経由で取得
      const user = await graphClient.api('/me').get();

      // ユーザーアイコンをMicrosoft Graph経由で取得(アイコンを設定していない場合は404になる)
      const photo = await graphClient.api('/me/photo/$value').get();

      const parent = document.getElementById('contents');

      // 取得したユーザー情報をHTML上に描画
      if (photo) {
        const photoEl = document.createElement('img');
        const url = (window.URL || window.webkitURL).createObjectURL(photo);
        photoEl.setAttribute('src', url);
        parent.appendChild(photoEl);
      }
      const userInfoEl = document.createElement('label');
      const content = document.createTextNode(user.displayName);
      userInfoEl.appendChild(content);
      userInfoEl.setAttribute('class', 'user-info');

      parent.appendChild(userInfoEl);
    }

    btn.addEventListener('click', loginAction);
  </script>
</html>
```

見ていただいたとおり、効力としてはあまり変化は感じられません。

これが強力な効果を発揮するのはJavascript上の処理比重が高く、フロントのバックグラウンドでGraphを使用した処理がゴリゴリと実装されるときだと思います。

データの表示ではなく登録を行いたい場合など、Javascriptでゴリゴリ書く必要があるものなどです。

(登録は[登録できるComponentが提案されている](https://github.com/microsoftgraph/microsoft-graph-toolkit/issues/1240)ようですが…

もしくはTypeScriptで実装している場合ですね。やりとりするデータの型が定義されているため、開発がしやすく、大規模な開発を行う上では大きな力になってくれると思います。

[Graph Tool Kitを利用してMicrosoft Graphを利用するWebアプリケーションを作る→](./4-using-toolkit)

# 参考

* [microsoftgraph/msgraph-sdk-javascript](https://github.com/microsoftgraph/msgraph-sdk-javascript)
* [Microsoft Graph を使った JavaScript の単一ページ アプリの作成](https://docs.microsoft.com/ja-jp/graph/tutorials/javascript)