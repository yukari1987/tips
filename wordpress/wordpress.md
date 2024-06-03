# ウィジェット

## ウィジェットを管理画面に表示させる
```
//function.php
function mytheme_widgets(){
  //ウィジェットエリアを登録
  register_sidebar(array('id' => 'sidebar-1','name'=>'フッターメニュー'));
}
add_action('widgets_init','mytheme_widgets');
```

## 管理画面から外観->ウィジェットを選択
```
//sidebar.php
//テーマfolder内のファイルのURLを出力
<?php get_theme_file_uri();?>

//記事投稿者のブログの表示名を出力
<?php the_author_meta('display_name');?>

//記事投稿者のプロフィール情報を出力
<?php the_author_meta('decription');?>
```