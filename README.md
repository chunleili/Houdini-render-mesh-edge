# Houdini-render-mesh-edge
思路：用vex增加polyline到prim当中。注意stage中sopimport 不要勾选导入N和P


![image](https://github.com/user-attachments/assets/8ce33609-63ca-41f7-9caa-3da0d5a2d0c3)


```cpp
float line_width = chf("line_width"); // 从参数获取线条宽度
int new_prim;
int v0, v1;

// 获取总图元数量
int prim_count = nprimitives(0);

// 遍历所有图元
for (int primnum = 0; primnum < prim_count; primnum++) {
    // 跳过非三角形面
    if (primvertexcount(0, primnum) != 3) continue;
    
    // 获取当前图元的顶点
    int verts[] = primpoints(0, primnum);
    
    // 为每个边创建一个新的polyline
    for (int i = 0; i < 3; i++) {
        v0 = verts[i];
        v1 = verts[(i + 1) % 3];
        
        // 创建新的polyline
        new_prim = addprim(0, "polyline");
        
        // 添加顶点到polyline
        int vtx0 = addvertex(0, new_prim, v0);
        int vtx1 = addvertex(0, new_prim, v1);

        setprimattrib(0, "width", new_prim, line_width); // 线条宽度
    }
}
```


用name分组出line和ball
![image](https://github.com/user-attachments/assets/8a69dc0c-4449-41b4-801c-050fb02d5011)

