##横スクロールの実装(未確認)
```
(function($) {

	new horizontalScroll('.js-horizontalScroll',{
		mediaQuery    : 'screen and ( min-width: 1024px )',
		direction     : 'left to right',
		innerSelector : '.js-horizontalScroll__inner',
		speed         : .2,
		speed         : .1,
		scrollbar     : false,
		sticky        : true,
		stickySelector: '.l-header',
	});
})
  ```