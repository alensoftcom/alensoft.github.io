### Задача Выбора Действий (Activity Selection Problem)

#### Введение

Задача выбора заданий формально определена следующим образом: дано множество заданий, каждое из которых имеет время
начала \( s_i \) и время окончания \( f_i \), необходимо выбрать максимальное количество непересекающихся заданий. Два
задания считаются непересекающимися, если время начала одного не предшествует времени окончания другого.

#### Подход

1. **Сортировка Действий по Времени Окончания**:
    - Ключевая идея состоит в выборе действия, которое заканчивается ранее всех, чтобы максимально использовать время для
      других действий.

2. **Выбор Действий**:
    - Начинаем с первого действия в отсортированном списке.
    - Выбираем следующее действие, которое начинается после или в то же время, когда предыдущее действие заканчивается.

#### Пример

Рассмотрим следующие действия:

- Действие A: начинается в 1, заканчивается в 3
- Действие B: начинается в 2, заканчивается в 4
- Действие C: начинается в 3, заканчивается в 5
- Действие D: начинается в 0, заканчивается в 6
- Действие E: начинается в 5, заканчивается в 7

**Пошаговое Решение**:

1. **Сортировка Действий по Времени Окончания**:
    - A: заканчивается в 3
    - B: заканчивается в 4
    - C: заканчивается в 5
    - D: заканчивается в 6
    - E: заканчивается в 7

2. **Выбор Действий**:
    - Начинаем с Действия A (заканчивается в 3).
    - Затем выбираем Действие C (начинается в 3, заканчивается в 5).
    - Наконец, выбираем Действие E (начинается в 5, заканчивается в 7).

**Выбранные Действия**: A, C, E

#### Реализация Кода

**Java**

```java
import java.util.Arrays;
import java.util.Comparator;

class Activity implements Comparable<Activity> {
    int start;
    int end;

    public Activity(int start, int end) {
        this.start = start;
        this.end = end;
    }

    @Override
    public int compareTo(Activity other) {
        return this.end - other.end;
    }
}

public class ActivitySelection {
    public static void main(String[] args) {
        Activity[] activities = {
                new Activity(1, 3),
                new Activity(2, 4),
                new Activity(3, 5),
                new Activity(0, 6),
                new Activity(5, 7)
        };

        Arrays.sort(activities);

        System.out.println("Выбранные действия:");
        int[] selected = selectActivities(activities);
        for (int i : selected) {
            System.out.println("Начало: " + activities[i].start + ", Конец: " + activities[i].end);
        }
    }

    public static int[] selectActivities(Activity[] activities) {
        int n = activities.length;
        if (n == 0) return new int[0];

        int[] selected = new int[n];
        selected[0] = 0;
        int count = 1;

        int currentEnd = activities[0].end;
        for (int i = 1; i < n; i++) {
            if (activities[i].start >= currentEnd) {
                selected[count++] = i;
                currentEnd = activities[i].end;
            }
        }

        return Arrays.copyOf(selected, count);
    }
}
```

**Python**

```python
class Activity:
    def __init__(self, start, end):
        self.start = start
        self.end = end


def select_activities(activities):
    activities.sort(key=lambda x: x.end)
    selected = [activities[0]]
    current_end = activities[0].end
    for activity in activities[1:]:
        if activity.start >= current_end:
            selected.append(activity)
            current_end = activity.end
    return selected


if __name__ == "__main__":
    activities = [
        Activity(1, 3),
        Activity(2, 4),
        Activity(3, 5),
        Activity(0, 6),
        Activity(5, 7)
    ]

    selected = select_activities(activities)
    print("Выбранные действия:")
    for act in selected:
        print(f"Начало: {act.start}, Конец: {act.end}")
```

#### Вычислительная Сложность

- **Сортировка**: O(n log n)
- **Итерация**: O(n)
- **Общая Сложность**: O(n log n)

Вычислительная сложность алгоритма выбора заданий определяется этапом сортировки, который занимает \( O(n \log
n) \) времени, где \( n \) является количеством заданий. Процесс выбора заданий сам по себе является линейным, \( O(
n) \), что делает общую временную сложность \( O(n \log n) \).

#### Крайние Случаи

1. **Все Действия Пересекаются**:
    - Пример: Действия 1-10, 2-9, 3-8
    - Можно выбрать только одно действие.

2. **Действия Не Пересекаются**:
    - Пример: Действия 1-2, 3-4, 5-6
    - Можно выбрать все действия.

3. **Действия с Одинаковым Временем Начала и Окончания**:
    - Пример: Действия 1-2, 1-2, 1-2
    - Можно выбрать только одно действие.

4. **Действия, Начинающиеся во Время Окончания Другого**:
    - Пример: Действия 1-3, 3-5
    - Оба действия можно выбрать, так как они не пересекаются.


## Заключение

Задача выбора заданий является примером задач, которые могут быть эффективно решены с помощью жадных алгоритмов
с временем выполнения \( O(n \log n) \).


[//]: # (Сценарий для ручного тестирования.)
[//]: # (Задача Выбора Действий &#40;Activity Selection Problem&#41;)
[//]: # (Консольный вариант.)
[//]: # ()
[//]: # (Вводится в консоль число задач.)
[//]: # (Затем парами будем вводить в консоль начало и конец задачи.)
[//]: # (На выводе ожидаем число принятых к выполнению задач.)
[//]: # (Нужно 10 разных случаев ввода и вывода тестирующих разные, обычные и граничные случаи)


### Примеры тестовых случаев для Задачи Выбора Действий

#### Тест 1: Одна Задача
- **Ввод:**
  ```
  1
  1 2
  ```
- **Ожидаемый Вывод:**
  ```
  1
  ```

#### Тест 2: Две Не Пересекающиеся Задачи
- **Ввод:**
  ```
  2
  1 2
  3 4
  ```
- **Ожидаемый Вывод:**
  ```
  2
  ```

#### Тест 3: Две Пересекающиеся Задачи
- **Ввод:**
  ```
  2
  1 3
  2 4
  ```
- **Ожидаемый Вывод:**
  ```
  1
  ```

#### Тест 4: Одна Задача Внутри Другой
- **Ввод:**
  ```
  2
  1 4
  2 3
  ```
- **Ожидаемый Вывод:**
  ```
  1
  ```

#### Тест 5: Задача с Началом и Концом в Один Момент
- **Ввод:**
  ```
  1
  1 1
  ```
- **Ожидаемый Вывод:**
  ```
  1
  ```

#### Тест 6: Множество Задач с Одинаковыми Временными Интервалами
- **Ввод:**
  ```
  3
  1 2
  1 2
  1 2
  ```
- **Ожидаемый Вывод:**
  ```
  1
  ```

#### Тест 7: Задачи с Началом, Совпадающим с Концом Предыдущей
- **Ввод:**
  ```
  2
  1 2
  2 3
  ```
- **Ожидаемый Вывод:**
  ```
  2
  ```

#### Тест 8: Несколько Пересекающихся и Непересекающихся Задач
- **Ввод:**
  ```
  4
  1 3
  2 4
  3 5
  4 6
  ```
- **Ожидаемый Вывод:**
  ```
  3
  ```

#### Тест 9: Нет Задач
- **Ввод:**
  ```
  0
  ```
- **Ожидаемый Вывод:**
  ```
  0
  ```

#### Тест 10: Задача с Началом После Конца
- **Ввод:**
  ```
  1
  3 2
  ```
- **Ожидаемый Вывод:**
  ```
  0
  ```

### Объяснение

- **Тест 1** проверяет базовый случай с одной задачей.
- **Тест 2** проверяет случай с двумя непересекающимися задачами.
- **Тест 3** проверяет случай с двумя пересекающимися задачами.
- **Тест 4** проверяет, когда одна задача полностью содержится внутри другой.
- **Тест 5** проверяет задачу, которая занимает нулевой временной интервал.
- **Тест 6** проверяет, когда все задачи имеют одинаковые интервалы времени.
- **Тест 7** проверяет граничный случай, когда задачи соприкасаются в точке времени.
- **Тест 8** проверяет более сложный случай с множеством задач, часть из которых пересекается.
- **Тест 9** проверяет граничный случай с нулевым количеством задач.
- **Тест 10** проверяет случай с некорректными временными интервалами, где начало после конца.

Эти тесты помогут убедиться, что программа корректно обрабатывает различные сценарии, включая обычные и граничные случаи.

