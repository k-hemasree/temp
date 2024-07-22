import heapq
class Node:
    def __init__(self,state,parent=None,cost=0,heuristic=0):
        self.state=state
        self.parent=parent
        self.cost=cost
        self.heuristic=heuristic
        
    def total_cost(self):
        return self.cost+self.heuristic
    
def astar_search(start_state,goal_state,neighbors_fn,heuristic_fn):
    open_set=[]
    closed_set=set()
    start_node=Node(start_state,cost=0,heuristic=heuristic_fn(start_state))
    heapq.heappush(open_set,(start_node.total_cost(),id(start_node),start_node))
    while open_set:
        _,_,current_node=heapq.heappop(open_set)
        if current_node.state==goal_state:
            path=[]
            while current_node:
                path.append(current_node.state)
                current_node=current_node.parent
            return path[::-1]
        closed_set.add(current_node.state)
        for neighbor_state in neighbors_fn(current_node.state):
            if neighbor_state in closed_set:
                continue
            neighbor_node=Node(neighbor_state)
            neighbor_node.parent=current_node
            neighbor_node.cost=current_node.cost+1
            neighbor_node.heuristic=heuristic_fn(neighbor_state)
            if any (neighbor_node.state==node.state for _,_, node in open_set):
                continue
            heapq.heappush(open_set,(neighbor_node.total_cost(),id(neighbor_node),neighbor_node))
    return None

def neighbors(state):
    x,y=state
    movements=[(0,1),(0,-1),(1,0),(-1,0)]
    return  [(x+dx,y+dy) for dx,dy in movements]

def heuristic(state):
    x,y=state
    goal_x,goal_y=4,4 
    return abs(x-goal_x)+abs(y-goal_y)

start_state=(0,0)
goal_state=(4,4)

Path=astar_search(start_state,goal_state,neighbors,heuristic)
print("Path:",Path)
                
