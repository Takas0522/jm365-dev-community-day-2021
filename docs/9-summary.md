[←mgt-getを利用する](./8-use-mgt-get.md)

今回のハンズオンの集大成です。ActiveDirectry内のユーザーの情報の一覧をBootstrapのカード形式で表示してみましょう。任意のタイミングで更新できるようにボタンによる画面の更新も実装してみます。

# 1.アクセス許可の確認

まずはユーザーのアクセス許可の確認です。[ユーザー一覧を表示する](https://docs.microsoft.com/ja-jp/graph/api/user-list?view=graph-rest-1.0&tabs=http)のに必要なアクセス許可を確認し、必要なアクセス許可を付与します。

# 2.Componentが存在するか確認

AAD内のユーザーの一覧が表示されるようなコンポーネントはないようなので(`mgt-people`をいじればいけるかもしれませんが…)ここは`mgt-get`を利用します。

# 3.動作確認

`mgt-get`でユーザーの一覧が参照できるか確認します。

下記コードでユーザー情報がコンソール上に表示されるか確認してみましょう

``` html
<html>
  <head>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.4/dist/umd/popper.min.js" integrity="sha384-q2kxQ16AaE6UbzuKqyBE9/u/KzioAlnx2maXQHiDX9d4/zp8Ok3f+M7DPm+Ib6IU" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.min.js" integrity="sha384-pQQkAEnwaBkjpqZ8RU1fF1AKtTcHJwFl3pblpTlHXybJjHpMYo79HY3hIi4NKxyj" crossorigin="anonymous"></script>
    <script src="https://unpkg.com/@microsoft/mgt/dist/bundle/mgt-loader.js"></script>
  </head>
  <body>
    <mgt-msal2-provider client-id="<ClientId>"
      authority="https://login.microsoftonline.com/<TenantId>/"></mgt-msal2-provider>
    <mgt-get resource="users" scopes="User.ReadBasic.All">
      <template data-type="default">
        {{console.log(this)}}
      </template>
    </mgt-get>
  </body>
</html>
```

4. 内部でユーザー情報取得&表示処理

`data-for`を利用してユーザー情報を1件ずつ取得していきます。ピックアップされたユーザーで`mgt-person`を利用してユーザー情報を表示します。

``` html
<html>
  <head>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.7.1/font/bootstrap-icons.css">
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.4/dist/umd/popper.min.js" integrity="sha384-q2kxQ16AaE6UbzuKqyBE9/u/KzioAlnx2maXQHiDX9d4/zp8Ok3f+M7DPm+Ib6IU" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.min.js" integrity="sha384-pQQkAEnwaBkjpqZ8RU1fF1AKtTcHJwFl3pblpTlHXybJjHpMYo79HY3hIi4NKxyj" crossorigin="anonymous"></script>
    <script src="https://unpkg.com/@microsoft/mgt/dist/bundle/mgt-loader.js"></script>
  </head>
  <body>
    <mgt-msal2-provider client-id="<ClientId>"
      authority="https://login.microsoftonline.com/<TenantId>/"></mgt-msal2-provider>
    <mgt-get resource="users" scopes="User.ReadBasic.All">
      <template data-type="default">
        <div data-for="user in this.value">
          <mgt-person person-query="{{this.user.mail}}"></mgt-person>
        </div>
      </template>
    </mgt-get>
  </body>
</html>
```

5. テンプレートカスタマイズ

ユーザー情報はBootstrapのカードで表示するのでテンプレートのカスタマイズを使用します。

イメージがない場合、何も表示されないのは悲しいので、適当にIconを表示します。

``` html
<html>
  <head>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.4/dist/umd/popper.min.js" integrity="sha384-q2kxQ16AaE6UbzuKqyBE9/u/KzioAlnx2maXQHiDX9d4/zp8Ok3f+M7DPm+Ib6IU" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.min.js" integrity="sha384-pQQkAEnwaBkjpqZ8RU1fF1AKtTcHJwFl3pblpTlHXybJjHpMYo79HY3hIi4NKxyj" crossorigin="anonymous"></script>
    <script src="https://unpkg.com/@microsoft/mgt/dist/bundle/mgt-loader.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.7.1/font/bootstrap-icons.css">
  </head>
  <body>
    <mgt-msal2-provider client-id="<ClientId>"
      authority="https://login.microsoftonline.com/<TenantId>/"></mgt-msal2-provider>
    <mgt-get resource="users" scopes="User.ReadBasic.All">
      <template data-type="default">
        <div data-for="user in this.value">
          <mgt-person person-query="{{this.user.mail}}">
            <template data-type="default">
              <div class="card" style="width: 18rem;">
                <img data-if="this.person.personImage" src="{{this.person.personImage}}" class="card-img-top">
                <svg data-else xmlns="http://www.w3.org/2000/svg" width="64" height="64" fill="currentColor" class="bi bi-x-octagon" viewBox="0 0 16 16">
                  <path d="M4.54.146A.5.5 0 0 1 4.893 0h6.214a.5.5 0 0 1 .353.146l4.394 4.394a.5.5 0 0 1 .146.353v6.214a.5.5 0 0 1-.146.353l-4.394 4.394a.5.5 0 0 1-.353.146H4.893a.5.5 0 0 1-.353-.146L.146 11.46A.5.5 0 0 1 0 11.107V4.893a.5.5 0 0 1 .146-.353L4.54.146zM5.1 1 1 5.1v5.8L5.1 15h5.8l4.1-4.1V5.1L10.9 1H5.1z"/>
                  <path d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"/>
                </svg>
                <div class="card-body">
                  <h5 class="card-title">{{this.person.displayName}}</h5>
                  <p class="card-text">{{this.person.mail}}</p>
                </div>
              </div>
            </template>
          </mgt-person>
        </div>
      </template>
    </mgt-get>
  </body>
</html>
```


# 6.更新処理の実装

最後にボタンを押したときに更新が行われる処理を実装します。

``` html
<html>
  <head>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.4/dist/umd/popper.min.js" integrity="sha384-q2kxQ16AaE6UbzuKqyBE9/u/KzioAlnx2maXQHiDX9d4/zp8Ok3f+M7DPm+Ib6IU" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.min.js" integrity="sha384-pQQkAEnwaBkjpqZ8RU1fF1AKtTcHJwFl3pblpTlHXybJjHpMYo79HY3hIi4NKxyj" crossorigin="anonymous"></script>
    <script src="https://unpkg.com/@microsoft/mgt/dist/bundle/mgt-loader.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.7.1/font/bootstrap-icons.css">
  </head>
  <body>
    <button id="refresh-button">一覧更新</button>
    <mgt-msal2-provider client-id="<ClientId>"
      authority="https://login.microsoftonline.com/<TenantId>/"></mgt-msal2-provider>
    <mgt-get resource="users" scopes="User.ReadBasic.All">
      <template data-type="default">
        <div data-for="user in this.value">
          <mgt-person person-query="{{this.user.mail}}">
            <template data-type="default">
              <div class="card" style="width: 18rem;">
                <img data-if="this.person.personImage" src="{{this.person.personImage}}" class="card-img-top">
                <svg data-else xmlns="http://www.w3.org/2000/svg" width="64" height="64" fill="currentColor" class="bi bi-x-octagon" viewBox="0 0 16 16">
                  <path d="M4.54.146A.5.5 0 0 1 4.893 0h6.214a.5.5 0 0 1 .353.146l4.394 4.394a.5.5 0 0 1 .146.353v6.214a.5.5 0 0 1-.146.353l-4.394 4.394a.5.5 0 0 1-.353.146H4.893a.5.5 0 0 1-.353-.146L.146 11.46A.5.5 0 0 1 0 11.107V4.893a.5.5 0 0 1 .146-.353L4.54.146zM5.1 1 1 5.1v5.8L5.1 15h5.8l4.1-4.1V5.1L10.9 1H5.1z"/>
                  <path d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"/>
                </svg>
                <div class="card-body">
                  <h5 class="card-title">{{this.person.displayName}}</h5>
                  <p class="card-text">{{this.person.mail}}</p>
                </div>
              </div>
            </template>
          </mgt-person>
        </div>
      </template>
    </mgt-get>
  </body>
  <script>
    const button = document.getElementById('refresh-button');
    button.addEventListener('click', () => {
      const getEl = document.querySelector('mgt-get');
      getEl.refresh();
    });
  </script>
</html>
```