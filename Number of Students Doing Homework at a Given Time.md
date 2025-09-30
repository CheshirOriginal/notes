https://leetcode.com/problems/number-of-students-doing-homework-at-a-given-time/description/
### Условие

Даны два целочисленных массива startTime и endTime и дан целый массив queryTime.
Ученик i начал выполнять домашнее задание в момент времени startTime[i] и закончил его в момент времени endTime[i].
Возвращает количество студентов, выполняющих домашнее задание в момент времени queryTime. Более формально, возвращает количество студентов, когда queryTime находится в интервале [startTime[i], endTime[i]] включительно.
### Пример
**Ввод:** startTime = [1,2,3], endTime = [3,2,7], queryTime = 4
**Выход:** 1

**Ввод:** startTime = [4], endTime = [4], queryTime = 4
**Выход:** 1
### Решение
```go
func busyStudent(startTime []int, endTime []int, queryTime int) int {
    res := 0
  
    for i := 0; i < len(startTime); i++ {
        if  startTime[i] <= queryTime && queryTime <= endTime[i] {
            res++
        }
    }
    return res
}
```