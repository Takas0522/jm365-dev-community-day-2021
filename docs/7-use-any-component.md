[←Componentをカスタマイズする](./6-custom-component.md)

# すこし凝ったテンプレートの組み立て方

## 配列データの表示

[data-for](https://docs.microsoft.com/ja-jp/graph/toolkit/customize-components/templates#looping)を使用することで、テンプレートないでデータ配列数分ループが行えます。

イベントの一覧など、List形式のデータを表示する場合によく利用します。

例えば下記は予定表データのタイトルと予定に参加する複数人のデータをリスト形式で表示します。

``` html
<html>
  <head>
    <script src="https://unpkg.com/@microsoft/mgt/dist/bundle/mgt-loader.js"></script>
  </head>
  <body>
    <mgt-msal2-provider client-id="d7418163-08b4-4d5f-941f-45897951f01d"
      authority="https://login.microsoftonline.com/1c4e72e7-52a1-475f-b469-0d4fbf3eae2e/"></mgt-msal2-provider>
    <mgt-agenda date="2021-11-07">
      <template data-type="event">
        {{ event.subject }}
        <ul>
          <li data-for='attendee in event.attendees'>
            {{ attendee }}
          </li>
        </ul>
      </template>
    </mgt-agenda>
  </body>
</html>

```

### 条件付きのデータの表示

特定のデータのみ表示/非表示にしたい場合は[`data-if`/`data-else`](https://docs.microsoft.com/ja-jp/graph/toolkit/customize-components/templates#conditional-rendering)を利用します。

例えば下記はユーザーの画像情報が存在した場合表示し、ない場合は名前を表示しています。

``` html
<html>
  <head>
    <script src="https://unpkg.com/@microsoft/mgt/dist/bundle/mgt-loader.js"></script>
  </head>
  <body>
    <mgt-msal2-provider client-id="d7418163-08b4-4d5f-941f-45897951f01d"
      authority="https://login.microsoftonline.com/1c4e72e7-52a1-475f-b469-0d4fbf3eae2e/"></mgt-msal2-provider>
      <mgt-person person-query="<人情報>">
        <template>
          <div data-if="person.image">
            <img src="{{ person.image }}" />
          </div>
          <div data-else>
            {{ person.displayName }}
          </div>
        </template>
      </mgt-person>
  </body>
</html>
```

他にも様々なデータへのアクセスの仕方があるので[ドキュメント](https://docs.microsoft.com/ja-jp/graph/toolkit/customize-components/templates)を見ながら何ができるか確認を行うことが重要です。

[mgt-getを利用する→](./8-use-mgt-get.md)