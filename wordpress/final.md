# 最終確認事項
## 誤字脱字の確認<br><br>
## レスポンシブ確認<br><br>
## 実機確認（iphone/android）<br><br>
## テスト記事が入っていないか<br><br>
## htmlの構文確認<br>
(https://validator.w3.org/#validate_by_input+with_options)<br><br>
## wpのユーザーは適切か<br><br>
## sitemap確認<br>
```
/wp-sitemap.xml
```
## フォームのメール送信先は合っているか。送信テストをする<br><br>
## OGPの確認
```
//ogp設定の登録
<head prefix="og: https://ogp.me/ns#">

//アドレス
<meta property="og:url" content="<?php the_permalink(); ?>" />

//wepページの種類（frontpageがwebsite/それ以外がarticle）
<?php if(is_front_page()): ?>
  <meta property="og:type" content="website" />
<?php else: ?>
  <meta property="og:type" content="article" />
<?php endif; ?>

//タイトル
<meta property="og:title" content="<?= wp_get_document_title(); ?>" />

//画像の設定(1200 / 630)
<meta property="og:image" content="<?= get_theme_file_uri('/img/ogp.jpg'); ?>" />
```

## ogp確認ツール<br>
(https://rakko.tools/tools/9/)<br><br>

## 検索エンジンでの表示確認
```
設定→表示設定→検索エンジンでの表示→検索エンジンがサイトをインデックスしないようにする
のチェックを外す
```
<br>

## googleアナリティクス設定
```
ページの閲覧数や国など解析できる。
googleアカウントからサイトごとに登録。
```


## google Search Console設定
```
chromeにサイトの状況を伝える。
検索ワード、順位を確認できる。
googleアカウントからサイトごとに登録。
DSN設定が必要。
設定が完了するまで１日ほどかかるので後日再確認する必要がある。
```

## Google reCaptcha設定
```
スパムメールのブロック。バージョンはv3に設定する。
googleアカウントからサイトごとに登録。
設定が完了するまで１日ほどかかるので後日再確認する必要がある。
```
## Google SEO対策マニュアル<br>
(https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=ja)

## SSLリダイレクト設定
```
//.htaccess
//httpにアクセスされた時もhttpsにリダイレクトする

//SSLリダイレクト設定
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

## スコア
・google fontsを使いすぎていないか <br>
・画像が大きすぎないか<br>
・WPで設定できる記事のアイコンで「中」を選択する<br>
・youtubeの読み込みをIntersectionObsercerAPIで遅らせる<br>
・reCAPTCHA（google）を問い合わせフォームのないページにも表示させていないか