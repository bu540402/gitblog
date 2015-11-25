title: hdu 4714  Tree2cycle
tags: []
date: 2013-09-09 14:06:21
---

<pre config="brush:cpp;toolbar:false;">/*题意：一个一棵树，去掉一条边的代价为1,加上一条边的代价也为1
 * 问用最小的代价使得这个树变成一个收尾相接的圆环。
 * 解法：先使得一棵树变成一条链。则出了首位点其他点的度都为2
 * 对于每个点来说如果其度为d且d&gt;2则需要去点的d-2条边。此时
 * 可以将问题分两种情况假设T(u)是树上以u为节点的子树。
 * 那么砍掉d-2条边时 
 * 情况1：砍掉的边都是与其子树相连的边，则此时形成一个以u为起点的链。如：/ 形状
 * 情况2：砍掉其父亲边，保留两条边与子节点相连。此时形成以u为中间节点的链。如 ^ 形状
 * 那么我们记录每个子树这两种情况下是的在子树中得到一条链的最小费用
 * f[u][1]与f[u][2];
 * 容易可以推知 f[u][2]&lt;=f[u][1]
 * 下面推出状态转移方程：
 * 情况一：若保留一条边与一个子节点相连，对于砍掉的边花费为f[v][1]+2
 * 那么若全部砍掉所有子节，其花费的和为sum。
 * 则f[u][1]=sum-max(f[v][1]+2-f[v][2]);
 * 情况二：此情况要保留两条边。则保留最大值与次大值就好了。
 * ps：该题目点数过多直接深搜为爆栈空间。在杭电上用c++可以加栈过关
 * 现场赛栈空间有限且不能加栈空间，所以需要把深搜改为循环模拟递归用栈来实现
 *
 */
//#pragma comment(linker, "/STACK:1024000000,1024000000")
#include &lt;iostream&gt;
#include &lt;cstring&gt;
#include &lt;algorithm&gt;
#include &lt;stack&gt;
#include &lt;cstdio&gt;
#include &lt;queue&gt;
using namespace std;
#define vN 1000101
#define vE 100001*2
#define inf 0x3fffffff
struct edge
{
    int v, next;
} e[vN * 2];
int head[vN], used[vN], en = 0;
int dp[vN][2];
void addedge(int x, int y)
{
    e[en].v = y;
    e[en].next = head[x];
    head[x] = en++;
    e[en].v = x;
    e[en].next = head[y];
    head[y] = en++;
}
stack&lt;int&gt; st;
int father[vN];
void dfss(int x)
{
    st.push(x);
    father[x] = 0;
    while (!st.empty())
    {
        int u = st.top();
        if (used[u])
        {
            int fmax = 0, smax = 0;
            int sum = 0;
            for (int i = head[u]; i != -1; i = e[i].next)
            {
                int v = e[i].v;
                if (v == father[u])
                    continue;
                sum += dp[v][1] + 2;
                int temp = dp[v][1] + 2 - dp[v][0];
                if (temp &gt; smax)
                {
                    smax = temp;
                    if (smax &gt; fmax)
                    {
                        int tp = fmax;
                        fmax = smax;
                        smax = tp;
                    }
                }

            }
            dp[u][0] = sum - fmax;
            dp[u][1] = sum - fmax - smax;
            st.pop();
        }
        else
        {
            used[u] = 1;
            for (int i = head[u]; i != -1; i = e[i].next)
            {
                int v = e[i].v;
                if (!used[v])
                {
                    st.push(v);
                    father[v] = u;
                }

            }
        }
    }
}

int main()
{
    //freopen("data.in", "r", stdin);
    int t;
    scanf("%d", &amp;t);
    while (t--)
    {
        en = 0;
        int n;
        scanf("%d", &amp;n);
        memset(used, 0, sizeof(int) * (n + 1));
        memset(dp, 0, sizeof(int) * (n + 1));
        memset(head, -1, sizeof(int) * (n + 1));
        for (int i = 0; i &lt; n - 1; i++)
        {
            int x, y;
            scanf("%d%d", &amp;x, &amp;y);
            addedge(x, y);
        }
        dfss(1);
        printf("%d\n", min(dp[1][0], dp[1][1]) + 1);
    }

}</pre>