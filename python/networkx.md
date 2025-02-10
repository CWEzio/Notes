# sharp bits
## `floyd_warshall_numpy`
It should be note that for the return `dist_mat` of `floyd_warshall_numpy`, `dist_mat[i, j]` is the distance between `nodelist[i]` and `nodelist[j]`, not distance between node `i` and `j`. The `graph.nodelist` might not be ordered. If your node are from `0` to `num_node - 1`, you can sort the `nodelist` and then pass it to the function:
```python
# Get a sorted list of nodes (assuming node labels are integers)
nodelist = sorted(nx_graph.nodes())

# Compute the distance matrix with the specified nodelist
results_mat = nx.floyd_warshall_numpy(nx_graph, nodelist=nodelist)
```
Then `dist_mat[i, j]` would be the distance between node `i` and node `j`.