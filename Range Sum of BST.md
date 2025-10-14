https://leetcode.com/problems/range-sum-of-bst/description/
### Условие

Учитывая корневой узел двоичного дерева поиска и два целых числа low и high, вернуть сумму значений всех узлов со значением в диапазоне [low, high].
### Пример

![[Pasted image 20251014132519.png]]
**Ввод:** root = [10,5,15,3,7,null,18], low = 7, high = 15
**Выход:** 32
### Решение

```c++
class Solution {
public:
    int rangeSumBST(TreeNode* root, int low, int high) {
        if (!root) return 0;
        int sum = 0;
        // если текущее значение больше нижней границы диапозона
        // значит в левом поддереве могут находится значения, 
        // входящие в этот диапозон
        if (root->val > low) sum += rangeSumBST(root->left, low, high);
        // аналогично с верхней границей диапазона
        if (root->val < high) sum += rangeSumBST(root->right, low, high);
        // проверяем, что текущее значение входит в диапазон
        if (root->val >= low && root->val <= high) sum += root->val;
        return sum;
    }
};
```