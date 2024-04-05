#HTMLを繰り返し表示したい
```
//functions.php
add_shortcode( 'my_shortcode', function(){
  return '
//ここにコード
	';
} );
```

```
//page-$id.php
<?php
//ショートコードのテキストを取得
$shortcode_text = '[my_shortcode]';
$shortcode_output = apply_filters('the_content', $shortcode_text);
?>
<?= $shortcode_output?>
```