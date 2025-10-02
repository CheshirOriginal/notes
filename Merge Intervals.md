https://leetcode.com/problems/merge-intervals/description/
### Условие

Дан массив интервалов, где intervals[i] = [starti, endi], объединить все перекрывающиеся интервалы и вернуть массив неперекрывающихся интервалов, которые покрывают все интервалы во входных данных.
### Пример
**Ввод:** intervals = \[[1,3],[2,6],[8,10],[15,18]]
**Выход:** \[[1,6],[8,10],[15,18]]

**Ввод:** intervals = \[[2,3],[4,5],[6,7],[8,9],[1,10]]
**Выход:** \[[1,10]]
### Решение
```go
func merge(intervals [][]int) [][]int {
	// сортируем интервалы по началу
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    write := 0 // указатель кда записывать объединенный интервал
    // проходимся по интервалам
    for i := 0; i < len(intervals); i++ {
        start := intervals[i][0]
        end := intervals[i][1]
        // ищем последовательность интервалов, которые можно объеденить
        // (идем пока можно объеденить)
        for i+1 < len(intervals) && end >= intervals[i+1][0] {
            end = max(end, intervals[i+1][1])
            i++
        }
        // записываем найденую последовательность
        intervals[write][0] = start
        intervals[write][1] = end
        write++
    }

    return intervals[:write]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```