---
title: php生成Html文件
tags:
  - 生成Html
id: '756'
categories:
  - - 杂谈天下
date: 2012-09-21 09:31:53
---

\[codesyntax lang="php"\]

<?php
$row = array (
array (
"新闻标题1",
"新闻内容1"
),
array (
"新闻标题2",
"新闻内容2"
)
);
for ($index = 0; $index < sizeof($row); $index++) {
$title = $row\[$index\]\[0\];
$content = $row\[$index\]\[1\];
$path = "page" . $index . ".html";

$fp = fopen("tmp.html", "r"); //只读
$str = fread($fp, filesize("tmp.html")); //读取内容

$str = str\_replace("{title}", $title, $str);
$str = str\_replace("{content}", $content, $str);

fclose($fp);

$handle = fopen($path, "w");
fwrite($handle, $str);
fclose($handle);

echo "成功！";
}

//unlink("page2.html"); //删除文件
//mkdir("tmpdir"); //创建文件夹
?>

\[/codesyntax\]