import random
import time

class KruskalAlgo:
    def __init__(self, node_ip):
        self.node = node_ip
        self.gph = []  # edge and its respective weight matrix
        self.abs_p = [-1] * self.node
        self.node_rnk = [0] * self.node  # node and its corresponding rank list

    def add_edge(self, node1, node2, wt): # stores edges and its respective weight in a list
        self.gph.append([node1, node2, wt])

    def find(self, v):
        if self.abs_p[v] == -1:
            return v
        self.abs_p[v] = self.find(self.abs_p[v])  # path compression
        return self.abs_p[v]

    def union(self, node1_frm, node2_to):
        if self.node_rnk[node1_frm] > self.node_rnk[node2_to]: # rank of node1_frm higher
            self.abs_p[node1_frm] = node2_to
        elif self.node_rnk[node1_frm] < self.node_rnk[node2_to]: # rank of node2_to higher
            self.abs_p[node2_to] = node1_frm
        else:
            self.abs_p[node1_frm] = node2_to # both have same ranks
            self.node_rnk[node2_to] += 1

    def kruskal_algo(self):
        self.gph = sorted(self.gph, key=lambda wt_item: wt_item[2]) # sorts all the edges by weight in ascending order

        i, j, sum_wt = 0, 0, 0
        mst = []

        while i < self.node - 1:
            node1, node2, wt = self.gph[j]
            x = self.find(node1)
            y = self.find(node2)

            if x != y: # capture the edges if it does not form a cycle
                i += 1
                mst.append([node1, node2, wt])
                sum_wt += wt
                self.union(x, y)

            j += 1
        print(f"\nThe total weight of minimum spanning tree: {sum_wt} \n")
        return mst

    @staticmethod
    def ip_value(ip_node): # generates connected graph data for the algorithm
        max_edg = ip_node * (ip_node - 1) // 2
        value_gph = []
        while len(value_gph) < max_edg:
            node1 = random.randint(0, ip_node - 1)
            node2 = random.randint(0, ip_node - 1)
            if node1 != node2 and {node1, node2} not in [{edge[0], edge[1]} for edge in value_gph]:
                weight = random.randint(1, 50)
                value_gph.append([node1, node2, weight])
        return value_gph



def result():
    ip_node = 65 # input the number of edges
    edges = KruskalAlgo.ip_value(ip_node)
    obj_g = KruskalAlgo(ip_node) # Creates KruskalAlgo object and add edges
    value_gph = KruskalAlgo.ip_value(ip_node)
    for edge in value_gph:
        obj_g.add_edge(edge[0], edge[1], edge[2])
    print(f"Total Nodes: {ip_node}")
    print(f"Total Edges: {len(edges)}")

    start_time = time.time()

    result_mst = obj_g.kruskal_algo() # Finds and prints the Minimum Spanning Tree
    print("Minimum Spanning Tree:")
    for v1, v2, wt in result_mst:
        print(f"Edge: {v1} - {v2}, Weight: {wt}")

    end_time = time.time()
    total_time = end_time - start_time
    print("\nThe total time is:", total_time)

result()

