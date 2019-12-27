# Wordpress-
Обернуть картинки (тег img) в ссылку (тег a) с url самой картинки


## делает IMG тег анкором ссылки на картинку указанную в этом теге, чтобы её можно было увеличить и посмотреть.
add_filter( 'the_content', function( $content ){

  // пропускаем если в тексте нет картинок вообще...
  if( false === strpos( $content, '<img ') )
	return $content;

  if( ! is_main_query() || ! in_array( $GLOBALS['post']->post_type, ['post'] ) )
	return $content;

  $img_ex = '<img[^>]*src *= *["\']([^\'"]+)["\'][^>]*>';
  $content = preg_replace_callback( "~(?:<a[^>]+>\s*)$img_ex|($img_ex)~", function($mm){
	// пропускаем, если картинка уже со ссылкой
	if( empty($mm[2]) )
	  return $mm[0];

	return '<a href="'. $mm[3] .'">'. $mm[2] .'</a>';
  }, $content );

  return $content;
}, 5 );

Если нужно наоборот, удалить ссылку оборачивающую IMG тег, используем такую регулярку:
$content = preg_replace( "~<a[^>]+href[^>]+\.(?:jpg|jpeg|png|gif)[^>]+>\s*(<img[^>]+>)\s*<\/a>~", '$1', $content );
