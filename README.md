def cutTheTree(data, edges):
    n = len(data)
    adj = [[] for _ in range(n)]

    for u, v in edges:
        u -= 1
        v -= 1
        adj[u].append(v)
        adj[v].append(u)

    total = sum(data)
    subtree_sum = data[:]
    parent = [-1] * n
    order = []
    stack = [0]

    # Build traversal order, rooted at node 1
    while stack:
        node = stack.pop()
        order.append(node)

        for neighbor in adj[node]:
            if neighbor != parent[node]:
                parent[neighbor] = node
                stack.append(neighbor)

    # Calculate subtree sums from bottom to top
    for node in reversed(order):
        if parent[node] != -1:
            subtree_sum[parent[node]] += subtree_sum[node]

    # Find the minimum difference by cutting each parent-child edge
    answer = float('inf')

    for node in range(1, n):
        difference = abs(total - 2 * subtree_sum[node])
        answer = min(answer, difference)

    return answer


if _name_ == '_main_':
    n = int(input().strip())
    data = list(map(int, input().split()))

    edges = []
    for _ in range(n - 1):
        edges.append(list(map(int, input().split())))

    print(cutTheTree(data, edges))
