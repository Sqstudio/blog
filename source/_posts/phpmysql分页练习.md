---
title: php+mysql分页练习
tags: []
id: '732'
categories:
  - - 杂谈天下
date: 2012-08-28 09:36:32
---

php+mySql 分页练习 1.$url = $\_SERVER\['REQUEST\_URI'\]; 2.$page = $\_GET\[page\]; 3.SELECT \* FROM \`message\` limit $pageindex $pageSize   \[codesyntax lang="php"\]

<?
include ("conn.php");

$pageSize = 4;
$pageindex = "0,";

$total = mysql\_query("SELECT count(1) FROM \`message\`");
echo $total;
if ($\_GET\[page\]) {
$page = $\_GET\[page\];
$page = $page < 0 ? 0 : $page;
$page = $page >= $total ? $total -1 : $page;

$pageindex = $page \* $pageSize;
$pageindex .= ",";
}

$url = $\_SERVER\['REQUEST\_URI'\];
$url = parse\_url($url);
$url = $url\[path\];

echo "<a href=$url?page=" . ($page -1) . ">上一页</a>" . "&nbsp&nbsp<a href=$url?page=" . ($page +1) . ">下一页</a>";

$sql = "SELECT \* FROM \`message\` limit $pageindex $pageSize";
$r = mysql\_query($sql);

echo " 总页数" . $total . "<br><hr>";
while ($ss = mysql\_fetch\_array($r)) {
echo $ss\[user\] . "<br><hr>";
}

?>

\[/codesyntax\]