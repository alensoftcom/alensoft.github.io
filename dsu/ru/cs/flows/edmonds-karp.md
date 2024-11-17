---
title: Алгоритм Эдмондса-Карпа
draft: true
---

Данный алгоритм очень похож на [алгоритм
Форда-Фалкерсона](алгоритм_Форда-Фалкерсона "wikilink")
и отличается только тем, что мы будем пускать поток не по произвольному
увеличивающему пути, а по кратчайшему. Для этого просто будем вместо
$dfs$ использовать $bfs$.

Алгоритм остается корректным по тем же причинам, что и алгоритм
Форда-Фалкерсона. Остается только оценить время работы.

### Немного теории

{{ Утверждение |Название=Лемма о критическом ребре |Показать название=1
|Утверждение=Пусть P - кратчайший путь из $s$ в $t$, назовём ребро $(v,
u)$ критическим если $c(v, u)$ - минимальна на всем пути $P$. Покажем,
что ребро может становится критическим не более $O(V)$ раз.
|Доказательство=Рассмотрим критическое ребро $(v, u)$
принадлежащее кратчайшему пути $P$. Так как $P$ - кратчайший
путь $d_0(v) + 1 = d_0(u)$. После пускания потока вдоль $P$ ребро
$(v, u)$ пропадает из остаточной сети и что бы оно там появилось снова
мы должны пустить поток вдоль обратного ему ребра $(u, v)$. В данном
алгоритме мы всегда пускаем поток вдоль кратчайшего пути, а что бы
ребро $(u, v)$ принадлежало ему должно выполняться равенство:
$d_1(u) + 1 = d_1(v)$. По лемме о кратчайшем увеличивающем пути:
$d_0(u) \<= d_1(u)$, а значит: $d_0(v) = d_0(u) - 1 \<= d_1(u) - 1
= d_1(v) - 2 \\Rightarrow d_0(v) \<= d_1(v) - 2$, то есть расстояние
до $v$ строго увеличилось. Так как расстояние не может превышать $|V|$
ребро $(v, u)$ не может становиться критическим больше чем $O(V)$ раз.
}}

### Оценка времени работы

Каждое ребро может становиться критическим $O(V)$ раз. Суммарно все
ребра становятся критическими $O(VE)$ раз. После пускания потока
вдоль кратчайшего пути, хотя бы одно ребро становится критическим,
значит всего может быть не более $O(VE)$ увеличений потока. Поиск
кратчайшего пути мы делаем с помощью $bfs$ следовательно время
работы алгоритма $O(VE^2)$.