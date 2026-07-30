``` Java
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        orig=image[sr][sc]
        if orig == color:
            return image
        q=[]
        n=len(image)
        m=len(image[0])
        image[sr][sc]=color

        q.append((sr,sc))

        while q:
            i,j=q.pop(0)

            if i+1<n and image[i+1][j]==orig:
                image[i+1][j]=color
                q.append((i+1,j))
                
            if i-1>=0 and image[i-1][j]==orig:
                image[i-1][j]=color
                q.append((i-1,j))
            if j+1<m and image[i][j+1]==orig:
                image[i][j+1]=color
                q.append((i,j+1))
            if j-1>=0 and image[i][j-1]==orig:
                image[i][j-1]=color
                q.append((i,j-1))

        return image
        
```