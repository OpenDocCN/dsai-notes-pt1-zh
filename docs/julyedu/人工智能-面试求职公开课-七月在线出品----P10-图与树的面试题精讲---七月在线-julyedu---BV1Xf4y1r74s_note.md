# 人工智能—面试求职公开课（七月在线出品） - P10：图与树的面试题精讲 🧠🌳

在本节课中，我们将要学习图与树相关的核心面试题。课程将从图论简介开始，梳理常见考点，并通过一系列由浅入深的例题，讲解如何运用递归等核心思想解决二叉树和图论问题。最后，我们将对关键知识点进行总结。

## 图论简介 📊

图是一种抽象的数学结构，由**节点**和**边**组成。根据边是否有方向，图可分为**有向图**和**无向图**。

在计算机数据结构中，一种特殊的图是**树**。树是一种无环的无向图。其中，**有根树**（如并查集）和**二叉树**（特别是**二叉搜索树**）是经典结构。**堆**也是一种特殊的二叉树，常用于堆排序算法。

## 面试题总体分析 📝

对于图论，面试常考察以下方面：
*   **连通性**：判断两点间是否有路径、求连通分量等。
*   **割点与割边**：删除该点或边会使图由连通变为不连通。
*   **最小生成树**：如Kruskal算法。
*   **最短路问题**：包括单源最短路、任意两点间最短路等。
*   **搜索算法**：广度优先搜索（BFS）和深度优先搜索（DFS）。
*   **欧拉回路与哈密顿回路**：概念性考察。
*   **拓扑排序**：相对简单。

对于树，常考察以下方面：
*   **定义与判断**：判断是否为二叉树、二叉搜索树、平衡二叉树等。
*   **属性计算**：求树的最大/最小高度（深度）。
*   **最近公共祖先（LCA）**：求两个节点的最近公共祖先。

## 例题精讲 🧩

上一节我们梳理了图与树的核心考点，本节中我们将通过具体例题来深入理解解题思路。所有例题将围绕一个统一的递归框架展开，难度逐步递增。

### 例一：由遍历序列构造二叉树 🔨

LeetCode 105题。给定二叉树的**前序遍历**和**中序遍历**序列，构造出原始的二叉树。

**核心思路**：
前序遍历的第一个节点是根节点。在中序遍历序列中找到这个根节点，其左侧序列即为左子树的中序遍历，右侧为右子树的中序遍历。根据左右子树的长度，同样可以将前序遍历序列分割为左子树和右子树的前序遍历序列。这样就得到了两个子问题，递归构造左右子树即可。

**代码框架**：
```cpp
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    if (preorder.empty()) return nullptr;
    TreeNode* root = new TreeNode(preorder[0]); // 根节点
    // 在中序序列中找到根节点的位置
    auto it = find(inorder.begin(), inorder.end(), preorder[0]);
    int leftSize = it - inorder.begin();
    // 递归构造左右子树
    vector<int> leftPre(preorder.begin()+1, preorder.begin()+1+leftSize);
    vector<int> leftIn(inorder.begin(), it);
    vector<int> rightPre(preorder.begin()+1+leftSize, preorder.end());
    vector<int> rightIn(it+1, inorder.end());
    root->left = buildTree(leftPre, leftIn);
    root->right = buildTree(rightPre, rightIn);
    return root;
}
```
这个过程本质上是**前序遍历**的递归：先构造根节点，再构造左子树，最后构造右子树。

**思考题**：LeetCode 106题，给定**后序遍历**和**中序遍历**序列构造二叉树，思路完全一致。

### 例二：二叉树问题的递归框架 🔄

从例一可以看出，二叉树问题大多可以用递归解决。递归的核心在于处理**根节点**、**左子树**、**右子树**三部分，根据处理顺序的不同，对应了抽象意义上的前序、中序、后序遍历。

以下是几个可用此框架解决的经典问题：

**1. 二叉树中的最大路径和 (LeetCode 124)**
问题：二叉树每个节点有一个整数值，求路径（节点序列）上各节点值之和的最大值。路径至少包含一个节点，且不一定经过根节点。
**思路**：对于每个节点，计算从该节点向下延伸（向叶子方向）的最大路径和。全局最大路径和可能出现在：1) 左子树向下延伸的路径；2) 右子树向下延伸的路径；3) 从左子树经过根节点再到右子树的“V”形路径。这是一个**后序遍历**的过程（先算左右，再算根）。
```cpp
int maxPathSum(TreeNode* root) {
    int result = INT_MIN;
    helper(root, result);
    return result;
}
int helper(TreeNode* root, int &result) {
    if (!root) return 0;
    int left = max(0, helper(root->left, result)); // 只取正值
    int right = max(0, helper(root->right, result));
    // 计算经过当前根节点的“V”形路径和，更新全局最大值
    result = max(result, left + right + root->val);
    // 返回从当前节点向下延伸的最大路径和（只能选择一边）
    return max(left, right) + root->val;
}
```

**2. 二叉树的最小深度 (LeetCode 111)**
问题：从根节点到最近**叶子节点**的最短路径上的节点数量。
**思路**：需要确保终点是叶子节点。递归计算左右子树的最小深度，但需处理子树为空的情况。
```cpp
int minDepth(TreeNode* root) {
    if (!root) return 0;
    if (!root->left && !root->right) return 1;
    int min_depth = INT_MAX;
    if (root->left) {
        min_depth = min(min_depth, minDepth(root->left));
    }
    if (root->right) {
        min_depth = min(min_depth, minDepth(root->right));
    }
    return min_depth + 1;
}
```

**3. 平衡二叉树的判断 (LeetCode 110)**
问题：判断二叉树是否是高度平衡的（每个节点的左右子树高度差不超过1）。
**思路**：在递归计算节点高度的同时，判断其左右子树是否平衡。可视为一种**后序遍历**。
```cpp
bool isBalanced(TreeNode* root) {
    bool balanced = true;
    height(root, balanced);
    return balanced;
}
int height(TreeNode* root, bool &balanced) {
    if (!root || !balanced) return 0;
    int left_h = height(root->left, balanced);
    int right_h = height(root->right, balanced);
    if (abs(left_h - right_h) > 1) balanced = false;
    return max(left_h, right_h) + 1;
}
```

**4. 二叉树的最大深度 (LeetCode 104)**
问题：求二叉树的**最大**深度（高度）。
**思路**：根据定义直接递归。空树深度为0，否则为左右子树最大深度加1。
```cpp
int maxDepth(TreeNode* root) {
    return root ? 1 + max(maxDepth(root->left), maxDepth(root->right)) : 0;
}
```

**5. 相同的树 (LeetCode 100)**
问题：判断两棵二叉树是否完全相同。
**思路**：递归判断根节点值是否相等，且左右子树是否分别相同。
```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
    if (!p && !q) return true;
    if (!p || !q) return false;
    return (p->val == q->val) && isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
}
```

**6. 对称二叉树 (LeetCode 101)**
问题：判断二叉树是否镜像对称。
**思路**：转化为判断两棵树（原树的左右子树）是否镜像。镜像判断：根节点值相等，且树A的左子树与树B的右子树镜像，树A的右子树与树B的左子树镜像。
```cpp
bool isSymmetric(TreeNode* root) {
    return isMirror(root, root);
}
bool isMirror(TreeNode* t1, TreeNode* t2) {
    if (!t1 && !t2) return true;
    if (!t1 || !t2) return false;
    return (t1->val == t2->val) && isMirror(t1->left, t2->right) && isMirror(t1->right, t2->left);
}
```

**7. 验证二叉搜索树 (LeetCode 98)**
问题：判断二叉树是否是有效的二叉搜索树（BST）。
**思路**：BST的中序遍历序列是严格递增的。进行中序遍历，并记录上一个访问节点的值，比较当前节点值是否大于上一个值。
```cpp
bool isValidBST(TreeNode* root) {
    TreeNode* prev = nullptr;
    return inorder(root, prev);
}
bool inorder(TreeNode* node, TreeNode* &prev) {
    if (!node) return true;
    if (!inorder(node->left, prev)) return false;
    if (prev && node->val <= prev->val) return false;
    prev = node;
    return inorder(node->right, prev);
}
```

### 例三：二叉树与链表的转换 🔗

上一节我们介绍了二叉树问题的通用递归框架，本节中我们来看看二叉树与链表相互转换这一特殊应用。

**1. 二叉树展开为链表 (LeetCode 114)**
问题：将二叉树按前序遍历顺序展开为一个只有右子树的单链表（原地修改）。
**思路**：递归处理。将左子树展开后接到根的右侧，再将原右子树展开后接到当前链表的末尾。
```cpp
void flatten(TreeNode* root) {
    if (!root) return;
    flatten(root->left);
    flatten(root->right);
    TreeNode* temp = root->right;
    root->right = root->left;
    root->left = nullptr;
    // 找到当前右子树（即原左子树）的末尾
    TreeNode* cur = root;
    while (cur->right) cur = cur->right;
    cur->right = temp; // 接上原右子树
}
```

**2. 有序链表转换二叉搜索树 (LeetCode 109)**
问题：给定一个单链表，元素按升序排列，将其转换为高度平衡的二叉搜索树。
**思路（O(n log n)）**：每次用快慢指针找到链表中点作为根节点，递归构造左右子树。由于链表不能随机访问，找中点需要O(n)时间。
**优化思路（O(n)）**：利用中序遍历的顺序性。先计算链表长度，然后递归构建。在递归过程中，通过引用传递链表节点指针，使其自然按中序顺序前进。
```cpp
TreeNode* sortedListToBST(ListNode* head) {
    int len = 0;
    ListNode* cur = head;
    while (cur) { len++; cur = cur->next; }
    return build(head, 0, len - 1);
}
TreeNode* build(ListNode* &head, int start, int end) {
    if (start > end) return nullptr;
    int mid = start + (end - start) / 2;
    TreeNode* left = build(head, start, mid - 1); // 构建左子树，head会移动
    TreeNode* root = new TreeNode(head->val); // 当前head即为中点
    head = head->next;
    root->left = left;
    root->right = build(head, mid + 1, end); // 构建右子树
    return root;
}
```
**思考题**：LeetCode 108题，将**有序数组**转换为平衡二叉搜索树，由于数组支持随机访问，实现更为简单。

### 例四：图的深拷贝（克隆图） 🧬

LeetCode 133题（中等）。给定一个无向连通图的节点，返回该图的深拷贝。图中每个节点包含其邻居列表。图可能包含自环和重复边。

**核心思路**：由于图可能有环，直接递归复制会导致无限循环或重复创建。使用**深度优先搜索（DFS）** 配合**哈希映射（Map）** 来记录原节点与克隆节点的对应关系。
1.  从给定节点开始DFS。
2.  如果当前节点已在Map中，直接返回其克隆节点。
3.  否则，创建当前节点的克隆体，并**立即放入Map**（防止后续递归又创建它）。
4.  递归克隆所有邻居节点，并添加到克隆节点的邻居列表中。

```cpp
unordered_map<Node*, Node*> visited;
Node* cloneGraph(Node* node) {
    if (!node) return nullptr;
    if (visited.find(node) != visited.end()) {
        return visited[node]; // 已克隆，直接返回
    }
    Node* cloneNode = new Node(node->val); // 创建克隆节点
    visited[node] = cloneNode; // 关键：先放入映射，再处理邻居
    for (Node* neighbor : node->neighbors) {
        cloneNode->neighbors.push_back(cloneGraph(neighbor));
    }
    return cloneNode;
}
```

### 例五：棋盘上的路径问题（转化为欧拉路） ♟️

这是一个较难的问题。给定一个矩形棋盘和一些必须经过的指定点。从任意起点出发，移动规则：每次只能水平或竖直移动任意格，但移动方向必须与上一次垂直（即不能连续两次同向移动）。每个指定点只能经过一次。判断是否存在这样的路径。

**问题转化**：
1.  这看起来像一个**哈密顿路径问题**（访问所有节点各一次），是NP-Hard问题，直接求解困难。
2.  巧妙转化：将每个点的**行坐标（X）** 和**列坐标（Y）** 视为两类节点。每次移动（水平或垂直）相当于在X节点和Y节点之间连一条边。
3.  要求遍历所有指定点，等价于要遍历所有这些X-Y边各一次，且节点（坐标值）可重复访问。
4.  这就转化为了判断**无向图是否存在欧拉路径（一笔画）** 的问题。欧拉路径存在性有多项式时间判定准则：对于无向图，当且仅当奇度顶点（连接边数为奇数的顶点）的个数为0或2时，存在欧拉路径。

**解题步骤**：
1.  统计所有指定点的行坐标和列坐标，构建X节点集和Y节点集。
2.  每个指定点对应一条连接其X节点和Y节点的边。
3.  计算所有X节点和Y节点的度（连接的边数）。
4.  统计奇度节点的数量。若为0或2，则存在满足条件的路径；否则不存在。

**思考题：密码锁问题（Google面试题）**
一个4位密码锁，状态从`ABCD`可以变为`BCDE`，操作是：移除最高位A，剩余位左移（BCD变成新状态的高三位），然后在最低位填入任意数字E（0-9）。问：是否存在一种操作序列，能遍历所有`0000`到`9999`的一万个状态各恰好一次？
**提示**：构建一个图，节点是**三位数字**（000-999），共1000个节点。如果节点`ABC`的后两位`BC`与节点`BCD`的前两位`BC`相同，则它们之间有一条有向边，这条边恰好对应了一个四位数状态`ABCD`。遍历所有边各一次，即欧拉回路问题。可以证明该图存在欧拉回路。

## 课程总结 📚

本节课我们一起学习了图与树在面试中的核心考题与解题思路。

1.  **理解递归**：递归是解决树形结构问题的利器。核心在于定义好子问题（通常是左右子树），并在递归调用中处理好根节点。
2.  **树的遍历**：前序、中序、后序遍历不仅是基础操作，其递归思想更是解决复杂树问题的框架。同时需掌握非递归实现。
3.  **其他重要问题**：
    *   **最近公共祖先（LCA）**：有离线算法（如Tarjan）和在线算法（如倍增法）。
    *   **隐式图搜索**：许多问题（如例五）需要自己构建图模型，再应用BFS/DFS。这涉及到连通分量、强连通分量的求解。
    *   **拓扑排序**：用于有向无环图（DAG）的节点排序，教材中有标准算法。
4.  **图论建模**：最难的问题往往在于将实际问题抽象为合适的图论模型（如哈密顿路转化为欧拉路），这需要敏锐的洞察力和练习。

![](img/679194881ab894eba409154b7fb6e955_1.png)

![](img/679194881ab894eba409154b7fb6e955_2.png)

通过本课的学习，希望大家能够掌握图与树相关问题的核心解题模式，并具备将复杂问题转化为已知模型的能力。