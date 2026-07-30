```Java
class Solution {
        int maxA = 0;
    public int maxAreaOfIsland(int[][] grid) {

        if(grid == null || grid.length == 0) return 0;
        int m = grid.length;
        int n = grid[0].length;
        int area = 0;

        for(int r = 0; r < m; r++){
            for(int c = 0 ; c < n; c++){
                if(grid[r][c] == 1){
                    area = bfs(grid,r,c);
                    if(area > maxA) maxA = area;
                }
            }
        }
        return maxA;
    }

    private int bfs(int[][] grid , int r, int c){
        int m = grid.length;
        int n = grid[0].length;


        int[][] directions = {
            {1,0}, 
            {-1,0}, 
            {0,-1}, 
            {0,1}   
        };
        Queue<Integer> q = new LinkedList<>();

        q.offer(r*n+c);
        grid[r][c] = 0;
        int area = 0;

        while(!q.isEmpty()){
            int curr= q.poll();
            int row = curr/n;
            int col = curr%n;
            area++;

            for(int[] dir : directions){
                int nr = row+dir[0];
                int nc = col+dir[1];

                if(nr>=0 && nr<m && nc >=0 && nc < n && grid[nr][nc]==1){
                    q.offer(nr*n+nc);
                    grid[nr][nc] = 0;
                }

            }

        }
        return area;

    }

    private int dfs(int[][] grid , int r, int c){
        if(r<0 || r>= grid.length || c<0 || c>=grid[0].length || grid[r][c]==0){
            return 0;
        }
        grid[r][c] = 0;
        int area = 1;

        area += dfs(grid,r+1,c);
        area += dfs(grid,r-1,c);
        area += dfs(grid,r,c+1);
        area += dfs(grid,r,c-1);
        return area;
    }
}
```