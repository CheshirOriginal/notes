https://leetcode.com/problems/validate-binary-search-tree/
### Условие

Учитывая `root`двоичного дерева, _определить, является ли оно допустимым двоичным деревом поиска (BST)_ .

Действительный **BST** определяется следующим образом:

- Левый узел содержит только узлы с ключами, **строго меньшими, чем** ключ узла.
- Правое поддерево узла содержит только узлы с ключами, **строго большими, чем** ключ узла.
- И левое, и правое поддеревья также должны быть бинарными деревьями поиска.
### Пример
![[Pasted image 20251010171855.png]]
**Ввод:** root = [2,1,3]
**Выход:** true
![[Pasted image 20251010171912.png]]
**Ввод:** root = [5,1,4,null,null,3,6]
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
private:
	// вспомогательная рекурсивная функция
    bool isValid(TreeNode* root, long long low, long long high) {
	    // если узел равен nullptr, он действителен
        if (!root) return true;
        int curr = root->val;
        // проверяем, соответсвует ли текущий элемнт заданному
        // диапазону
        if (curr <= low || curr >= high) return false;
        // запускаем рекурсию для левого и правого поддерева,
        // сужая диапозоны 
        return isValid(root->left, low, curr) && isValid(root->right, curr, high);
    }
public:
    bool isValidBST(TreeNode* root) {
	    // запускаем вспомогательную рекурсивную функцию
        return isValid(root , LLONG_MIN , LLONG_MAX);
    }
};
```