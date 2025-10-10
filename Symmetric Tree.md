https://leetcode.com/problems/symmetric-tree/description/
### Условие

Дан корень двоичного дерева, проверьте, является ли оно зеркальным отражением самого себя (т.е. симметричен относительно своего центра).
### Пример
![[Pasted image 20251010130746.png]]
**Ввод:** root = [1,2,2,3,4,4,3]
**Выход:** true
![[Pasted image 20251010130810.png]]
**Ввод:** root = [1,2,2,null,3,null,3]
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
	// используется вспомогательная рекурсивная функция для 
	// проверки зеркальности поддеревьев
    bool isMirrorTree(TreeNode* l, TreeNode* r) {
	    // если один из узлов пустой, то второй должен быть таким же
        if (l == nullptr || r == nullptr) return l == r;
        // если они оба не пустые, сравниваем сначала их содержимое
        // потом симетричность их поддеревьев (левое с правым)
        // и наоборот
        return (l->val == r->val) && isMirrorTree(l->left, r->right) && isMirrorTree(l->right, r->left);
    }
    
    bool isSymmetric(TreeNode* root) {
	    // запускаем вспомогательную функцию
        return isMirrorTree(root->left, root->right);
    }
};
```