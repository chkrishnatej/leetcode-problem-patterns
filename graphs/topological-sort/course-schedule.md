# Course Schedule

## Problem

[https://leetcode.com/problems/course-schedule/description/](https://leetcode.com/problems/course-schedule/description/)

## Intuition



## Time and Space Complexity



## Code:

```java
class TopologicalSortDFS {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Time complexity: O(V+E)
        // V --> num of Courses ||  E --> prerequisites
        
        // Set adjacency list
        Map<Integer, List<Integer>> graph = new HashMap<>();
        
        // Add contianers/dependent list for the courses
        for(int i=0;i<numCourses;i++)
            graph.put(i, new ArrayList<>());
        
        // Populate the graph
        for(int[] pre:prerequisites)            
            graph.get(pre[0]).add(pre[1]);
        
        // Maintain a visited array to keep track of course state
        // -1 --> Course is currently processing
        // 0 --> Course is not yet processed
        // 1 --> Course is processed successfully
        int[] visited = new int[numCourses];
        
        
        // Run the hasNoCycle on all course, there could be multiple components, so run using for loop
        for(int i=0;i<numCourses;i++)
            // If any cycle is encountered, it means, course schedule cannot be completed
            if(!hasNoCycle(graph, visited, i)) 
                return false;
        
        // If it has no cycles, the course schedule could be completed
        return true;
    }
    
    private boolean hasNoCycle(Map<Integer, List<Integer>> graph, int[] visited, int course) {
        // If course is encountered in current cycle, it has a cycle
        if(visited[course]==-1)
            return false;
        
        // Course completed processing, hence no cycle
        if(visited[course]==1)
            return true;
        
        visited[course]=-1; // Mark the course as currently processing
        
        // Run the hasNoCycle through all the dependents of current course
        for(int i:graph.get(course)) {
            if(!hasNoCycle(graph, visited, i)) // check if any cycles are present
                return false;
        }
        
        visited[course] = 1; // Mark the course to be successfully processed
        return true; // No cycle is present for current course
    }
}class TopologicalSortDFS {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Time complexity: O(V+E)
        // V --> num of Courses ||  E --> prerequisites
        
        // Set adjacency list
        Map<Integer, List<Integer>> graph = new HashMap<>();
        
        // Add contianers/dependent list for the courses
        for(int i=0;i<numCourses;i++)
            graph.put(i, new ArrayList<>());
        
        // Populate the graph
        for(int[] pre:prerequisites)            
            graph.get(pre[0]).add(pre[1]);
        
        // Maintain a visited array to keep track of course state
        // -1 --> Course is currently processing
        // 0 --> Course is not yet processed
        // 1 --> Course is processed successfully
        int[] visited = new int[numCourses];
        
        
        // Run the hasNoCycle on all course, there could be multiple components, so run using for loop
        for(int i=0;i<numCourses;i++)
            // If any cycle is encountered, it means, course schedule cannot be completed
            if(!hasNoCycle(graph, visited, i)) 
                return false;
        
        // If it has no cycles, the course schedule could be completed
        return true;
    }
    
    private boolean hasNoCycle(Map<Integer, List<Integer>> graph, int[] visited, int course) {
        // If course is encountered in current cycle, it has a cycle
        if(visited[course]==-1)
            return false;
        
        // Course completed processing, hence no cycle
        if(visited[course]==1)
            return true;
        
        visited[course]=-1; // Mark the course as currently processing
        
        // Run the hasNoCycle through all the dependents of current course
        for(int i:graph.get(course)) {
            if(!hasNoCycle(graph, visited, i)) // check if any cycles are present
                return false;
        }
        
        visited[course] = 1; // Mark the course to be successfully processed
        return true; // No cycle is present for current course
    }
}
```
