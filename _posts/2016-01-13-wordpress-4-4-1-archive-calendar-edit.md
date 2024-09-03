---
title: "WordPress 4.4.1 文章归档及站点日历年月格式修改"
date: "2016-01-13"
categories: 
  - "sometime-programmer"
tags: 
  - "wordpress"
  - "文章归档"
  - "漫步云端"
  - "站点日历"
---

其实文章归档年月格式修改网上可以找到很多，只是 Google 了一下，找不到站点日历年月格式修改的办法，所以就自己折腾了一下，做个记录吧。

#### WordPress 4.4.1 文章归档年月格式修改

* * *

打开 WordPress 4.4.1 目录内的 /wp-includes/general-template.php 文件，定位到 1629 行。 可以看到原来显示格式为 2016年一月 的代码： `$text = sprintf( __( '%1$s %2$d' ), $wp_locale->get_month( $result->month ), $result->year );` 我要的格式是 2016/01 就改成了： `$text = sprintf( __('%1$d/%2$s'), $result->year, zeroise($result->month, 2));`

#### WordPress 4.4.1 站点日历年月格式修改

* * *

编辑同一个文件，定位到 1854 行左右。 可以看到原来显示格式为 2016年一月 的代码： `/* translators: Calendar caption: 1: month name, 2: 4-digit year */ $calendar_caption = _x('%1$s %2$s', 'calendar caption'); $calendar_output = '<table id="wp-calendar"> <caption>' . sprintf( $calendar_caption, $wp_locale->get_month( $thismonth ), date( 'Y', $unixmonth ) ) . '</caption> <thead> <tr>';` 然尔我要的格式是 2016年01月，所以我改成了： `/* translators: Calendar caption: 1: month name, 2: 4-digit year */ $calendar_caption = _x('%1$s年%2$s月', 'calendar caption'); $calendar_output = '<table id="wp-calendar"> <caption>' . sprintf( $calendar_caption, date( 'Y', $unixmonth ),date( 'm', $unixmonth) ) . '</caption> <thead> <tr>';`

打完收功，记得保存一下，然后就可以刷新页面看到效果了。
