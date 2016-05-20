# Wordpress code snippets and hacks

A collection of code snippets and hacks for Wordpress and some of its plugins.

## Let WordPress manage the document title

Remove the `<title>` tag in your theme's `header.php`. It should look something like this:

```php
<title><?php wp_title(''); ?></title>
```

Then add this code in your theme's `functions.php`:

```php
  /*
   * Let WordPress manage the document title.
   * By adding theme support, we declare that this theme does not use a
   * hard-coded <title> tag in the document head, and expect WordPress to
   * provide it for us.
   */
  add_theme_support('title-tag');
```

## Change Anspress' pages titles while using Yoast SEO plugin

```php
 // Change WP SEO TITLE to make better Anspress' pages titles.
 add_filter('wpseo_title', 'change_wpseo_title', 10, 1);
 function change_wpseo_title($title) {
  $subject = $title;
  // Change the separator to your liking.
  $separator = "|";
  $pattern_get_before = "/^(.*?)\\". $separator ."/"; // /^(.*?)\|/
  $pattern_get_after = "/([^\\". $separator ."]+$)/"; // /([^\|]+$)/
  // Get everything before $separator.
  preg_match($pattern_get_before, $subject, $matches);
  $matches_get_before = $matches[1];
  // Get everything after $separator.
  preg_match($pattern_get_after, $subject, $matches);
  $matches_get_after = $matches[1];
  if(is_question()){ // Anspress' Question page
    // Create new title, edit to your liking.
    $new_title = $matches_get_before . $separator . $matches_get_after;
    return $new_title;
  } else if(is_ask()){ // Anspress' Ask page
    // Create new title, edit to your liking.
    $new_title = $matches_get_before . $separator . $matches_get_after;
    return $new_title;
  } else if(is_page(20)){ // Anspress' Base page (change the number for your Anspress' Base page ID)
    // Create new title, edit to your liking.
    $new_title = $matches_get_before . $separator . $matches_get_after;
    return $new_title;
  } else {
    return $title;
  }
}
```

## Change Anspress' questions descriptions while using Yoast SEO plugin

```php
// Change WP SEO DESCRIPTION to make better Anspress' questions descriptions.
add_filter('wpseo_metadesc', 'change_wpseo_description', 10, 1);
function change_wpseo_description($description) {
  if(is_question()){
    $q_ar = get_post(get_question_id());
    // Get the content of the question, remove all tags and trim it.
    $new_description = wp_trim_words( wp_strip_all_tags($q_ar->post_content), 150);
    return $new_description;
  } else {
    return $description;
  }
}
```
