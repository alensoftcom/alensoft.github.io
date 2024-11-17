---
title: Пересечение полуплоскостей
draft: true
---

Очень похожей задачей является пересечение полуплоскостей (англ. *half-plane intersection*). Требуется найти множество, удовлетворяющее набору неравенств:

$$
a_i x + b_i y + c \geq 0
$$

Например, через сведение к пересечению полуплоскостей можно (не самым оптимальным образом) строить подобные клёвые картинки:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Coloured_Voronoi_2D.svg/220px-Coloured_Voronoi_2D.svg.png)

Это *диаграмма Вороного* — для каждой точки выделено множество точек плоскости, которые являются к ней ближайшими. Чтобы построить это множество для данной точки $p_i$, можно перебрать все осталньные точки $p_j$, посчитать коэффициенты прямой, которая делит плоскость между ними пополам, и пересечь все ориентированные в нужную сторону полуплоскости, соответствующие этим прямым.

Результатом пересечения множества полуплоскостей тоже будет выпуклая оболочка — полуплоскость это выпуклое множество, а пересечение любых выпуклых множеств тоже выпуклое. Возможно, эта оболочка будет бесконечной по каким-то направлениям. Чтобы не обрабатывать это отдельно, часто добавляют *bounding box* — «коробку» из четырёх полуплоскостей на большом удалении.

Один из способов построения — построить отдельно верхнюю и нижнюю огибающую и пересечь. Для этого нужно разделить все полуплоскости на «смотрящие вверх» и «смотрящие вниз», для обеих групп отсортировать их по углу вектора нормали и пройтись по ним со стеком, удаляя «ненужные» полуплоскости — те, которые полностью покрываются новой и предпоследней полуплоскостью в стеке.

```c++
// TODO: я не хочу это кодить
```

Алгоритм почти совпадает с алгоритмом алгоритмом Грэхэма и тоже имеет асимптотику $O(n \log n)$. На самом деле, разделять полуплоскости на верхние и нижние и потом объединять не требуется — можно пройтись таким же образом по сортированному набору полуплоскостей два раза, и какой-то подотрезок получившегося стека будет нужным ответом. Автор не в состоянии внятно объяснять, почему.

### Нахождение касательной

В некоторых задачах нам на самом деле не нужно явно находить пересечение, а достаточно лишь определить, пустое оно или нет. В [блоге Петра Митричева](https://petr-mitrichev.blogspot.com/2016/07/a-half-plane-week.html) описан простой алгоритм, позволяющий это делать за линейное время.

* Если это так, то она останется той же, и ничего дальше считать не надо.

* Если это не так, то новая точка должна лежать на пересечении новой полуплоскости и какой-то из старых. Важный факт здесь в том, что она лежит на границе новой полуплоскости. Чтобы её найти, пересечём все старые полуплоскости с этой границей и решим одномерную задачу пересечения множества интервалов, бесконечных в одну из сторон. Если получившийся интервал пуст, то и пересечение полуплоскостей пустое, а в противном случае один из его концов будет искомой точкой, если его положить обратно на границу новой полуплоскости.

Теперь интересная часть — асимптотика. Казалось бы, такой алгоритм в худшем случае может работать за квадратичное время — мы могли каждый раз на $k$-том шаге пересекать все имеющиеся полуплоскости с новой за $O(k)$.

Выясняется, что за счёт рандомизированности нам нужно выполнять это пересечение не так уж и часто. В случайном множестве из $k$ полуплоскостей только две из них дают в пересечении оптимальную точку. Из этого следует, что вероятность того, что $k$-тая полуплоскость станет новой граничной (и понадобится всё пересчитывать) равна всего $\frac{2}{k}$.

Таким образом, *в среднем* алгоритм совершит линейное количество операций:

$$
\frac{2}{2} \cdot 2 + \frac{2}{3} \cdot 3 + \frac{2}{4} \cdot 4 + \ldots + \frac{2}{n} \cdot n = \sum_k \frac{2}{k} \cdot k = O(n)
$$

Подобная техника иногда используется и для других геометрических задач — например, для [нахождения пары ближайших точек на плоскости](https://www.cs.cmu.edu/~ckingsf/class/02713/lectures/lec31-random.pdf).


---

Заметим, что если у нас есть некоторая прямая, задаваемая уравнением $ax
+ by + c = 0$, то множества точек, удовлетворяющие неравенствам $ax + by
+ c \\leq 0$ и $ax + by + c \\geq 0$ это полуплоскости, на которые эта
прямая разделяет плоскость. Будем задавать поплоскости уравнениями $ax
+ by + c \\leq 0$, потому что для полуплоскостей второго вида можно
просто рассмотреть прямую с коэфициентами $-a, -b, -c$. Пусть у нас
задано много полуплоскостей. Мы хотим найти их пересечение, то есть
как-то описать множество точек, удовлетворяющее всем неравенствам.

Для простоты предположим сначала, что нету вертикальных прямых.
Рассмотрим множество прямых, полуплоскости которых "смотрит"
вних и полуплоскости которых "смотрят" вверх.

Пусть у нас есть множество полуплоскостей, которые "смотрят" в одну
сторону. Пусть для простоты они смотрят вверх. Тогда рассмотрим все
прямые, которые разделяют плоскость. Мы хотим найти такую верхнюю
огибающую всех прямых, то есть выбрать в каждой $x$ координате
максимальную точку с такой $x$ координатой, лежащую на одной из
прямых. Этот метод также используется в очень известном методе
[Convex hull trick](Convex_hull_trick "wikilink"). Попробуем повторить
что-то типо Алгоритма Грэхэма. Отсортируем все прямые по углу с осью
$oX$. Будем перебирать прямые в таком порядке и в стеке поддерживать
верхнюю огибающую для тех прямых, которые мы перебрали. Для каждой
прямой будем поддерживать минимальную $x$ координату, начиная с
которой эта прямая максимальная. При добавлении новой прямой мы
должны будем выкинуть из стека несколько последних прямых и добавить
новую. Пока стек непустой будем вынимать из него последнюю прямую.
Рассмотрим точку пересечения этой прямой с нашей. Если $x$
координата этого пересечения будет не больше чем минимальная
$x$ координата, начиная с которой последняя прямая из стека была
максимальной, то последняя прямая в стеке больше никогда не
будет максимальной и мы должны вынуть ее из стека. После этого надо
добавить эту прямую в конец стека и минимальная $x$ координата начиная с
которой эта прямая будет максимальной будет равна $x$ координате
последней точки пересечения (во время того как мы вынимали
последнюю прямую из стека).

Далее надо пересечь две цепочки: полуплоскостей смотрящих вниз и
смотрящих вверх. Для этого переберем все пары звеньев верхней и
нижней цепочки и попробуем их пересечь. Таким образом найдется $\\leq
2$ точек пересечения, которые надо будет добавить в фигуру пересечения и
удалить все звенья левее и правее добавленных точек.

Для того, чтобы разобраться с вертикальной прямой, можно попробовать
пересечь ее с полученной конструкцией (что очень неприятно), а можно
просто выбрать любую пару перпендикулярных векторов и сказать, что новые
направления осей это эти вектора и в зависимости от этого выбрать какие
полуплоскости смотрят вверх, а какие вниз.

Общее время работы составляет $O(n^2)$.