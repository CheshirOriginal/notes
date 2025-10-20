https://leetcode.com/problems/path-sum/description/
### Условие

Учитывая корень двоичного дерева и целочисленное значение targetSum, вернуть значение true, если дерево имеет путь от корня до листьев такой, что сложение всех значений вдоль пути равно targetSum.

Лист — это узел, не имеющий дочерних узлов.
### Пример
![[Pasted image 20251010140231.png]]
**Ввод:** root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
**Выход:** true

**Ввод:** root = [], targetSum = 0
**Выход:** false
### Решение

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
	    // если пустое дерево или достигли конца, не набрав targetSum
        if(root == nullptr) return false;
        // если текущий элемент это лист и путь до него (включая его)
        // равен targetSum
        if(root->left == nullptr && root->right == nullptr && targetSum == root->val) return true;
        // запускаем рекурсию
        const auto l = hasPathSum(root->left, targetSum - root->val);
        const auto r = hasPathSum(root->right, targetSum - root->val);
        // объеденяем ответы (если есть хотя бы одна true, она 
        // поднимется вверх)
        return l || r;
    }
};
```