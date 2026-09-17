## title: N-Queens Problem Solution Guide (LeetCode 51)

## Java Solution

```
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<List<String>> solveNQueens(int n) {
        Set<Integer> cols = new HashSet<>();
        Set<Integer> diag = new HashSet<>();
        Set<Integer> antiDiag = new HashSet<>();
        
        List<List<String>> res = new ArrayList<>();
        char[][] board = new char[n][n];
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = '.';
            }
        }
        
        backtrack(0, n, board, cols, diag, antiDiag, res);
        return res;
    }
    
    private void backtrack(int r, int n, char[][] board, Set<Integer> cols, Set<Integer> diag, Set<Integer> antiDiag, List<List<String>> res) {
        if (r == n) {
            res.add(constructBoard(board));
            return;
        }
        
        for (int c = 0; c < n; c++) {
            if (cols.contains(c) || diag.contains(r - c) || antiDiag.contains(r + c)) {
                continue;
            }
            
            cols.add(c);
            diag.add(r - c);
            antiDiag.add(r + c);
            board[r][c] = 'Q';
            
            backtrack(r + 1, n, board, cols, diag, antiDiag, res);
            
            cols.remove(c);
            diag.remove(r - c);
            antiDiag.remove(r + c);
            board[r][c] = '.';
        }
    }
    
    private List<String> constructBoard(char[][] board) {
        List<String> list = new ArrayList<>();
        for (char[] row : board) {
            list.add(new String(row));
        }
        return list;
    }
}

```