# Course Schedule II

## Problem

[https://leetcode.com/problems/course-schedule-ii/description/](https://leetcode.com/problems/course-schedule-ii/description/)

## Intution



## Time and Space complexity

## Code

{% tabs %}
{% tab title="DFS approach" %}
```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {     
        // V -> Vertices, E -> Edges
        // Time Complexity: O(V+E), have to iterate through all the vertices and edges
        // Space Complexity: O(V), in the worsr case the dfs might have to backtrack all the vertices  
        // Set up adjacency list
        Map<Integer, List<Integer>> graph = new HashMap<>();
        
        // Set up nodes/course
        for(int i=0;i<=numCourses;i++) {
            graph.put(i, new ArrayList<>());
        }
        
        // Populate directed graph
        for(int i=0;i<prerequisites.length;i++) {
            graph.get(prerequisites[i][1]).add(prerequisites[i][0]);
        }
        
        // Setup a tracker with three states
        /*
            0 -> Unprocessed
            1 -> Processed
            -1 -> Currently processing
        */
        int[] visited = new int[numCourses];
        
        // Set up a stack to get order of courses that is being processed
        Stack<Integer> stack = new Stack<>();
        
        // Set up a flag to represent if the course can be completed or not
        boolean possible = true;
        
        // Process the course
        for(int i=0;i<numCourses;i++) {
            if(!isValidSchedule(graph, visited, i, stack)) {
                possible = false;
                break;
            }
        }
        
        if(possible) {
        // If its possible then give the order of the course
        int[] orderOfCourses = new int[numCourses];
            int i=0;
            while(!stack.isEmpty()) {
             orderOfCourses[i++] = stack.pop();
            }
            return orderOfCourses;
        }
        
        return new int[]{};
    }

    private boolean isValidSchedule(Map<Integer, List<Integer>> graph, int[] visited, int course, Stack<Integer> stack) {
        // Handle cycles and previously processed course
        if(visited[course]!=0) {
            return visited[course] == 1;
        }
        
        // Mark the current course for processing 
        visited[course] = -1;
        
        // Check for the dependent courses have any cycles
        for(int nextCourse: graph.get(course))  {
            if(!isValidSchedule(graph, visited, nextCourse, stack)) {
                return false;
            }
        }
        
        stack.push(course); // Push the course to stack to maintain order
        visited[course] = 1; // Mark the course as processed completly
        return true;
    }
}
```
{% endtab %}

{% tab title="BFS (Kahns Algorithm) approach" %}
```java
class TopologicalSwortWithKahnsAlgorithm  {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // Time Complexity: O(V+E)
        // Space Complexity: O(V+E)
        // where V and E are the total number of vertices and edges in the graph, respectively.

        List<Integer>[] graph = new List[numCourses]; // A primitive array of type "List<Integer>"
        List<Integer> result = new ArrayList<>(); // Store procesed courses
        int[] inDegree = new int[numCourses]; // Track of incoming nodes
        
        for(int i=0;i<numCourses;i++) {
            graph[i] = new ArrayList<>(); // Pre populate graph
        }
        
        for(int[] p:prerequisites) { // Building graph
            int u = p[1];
            int v = p[0];
            graph[u].add(v);
            ++inDegree[v]; // Keeping track of indgree for "v"
        }
        
        Queue<Integer> q = new LinkedList<>();
        
        for(int i=0;i<numCourses;i++)
            if(inDegree[i]==0) // To get started, we need at least one course which does not have a prerequisite
                q.offer(i); 
        
        while(!q.isEmpty()) {
            int u = q.poll();  // Take out the course from "Q"
            result.add(u); // It is considered as processed, add to result
            
            for(int v:graph[u]) // Process the neighbours of "u", which is the courses which have "u" as a prerequsite
                /*
                Reduce the indegree as we are processing the nodes, 
                when indegree == 0, i.e. all the pre requsiite courses for "v" are processed
                */
                if(--inDegree[v] == 0) 
                    q.offer(v); // Then add it to "q"
        }
        
        if(numCourses!=result.size())
            return new int[]{};

        return result.stream().mapToInt(i->i).toArray();
    }
}
```
{% endtab %}
{% endtabs %}
