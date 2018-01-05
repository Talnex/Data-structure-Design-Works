```c++

void dfs(int a,int b){
    for (int i=0; i<NUM_POINTS; i++) {
        if (!matrix[a][i]) {
            visited[a][i]=1;
            visited[i][a]=1;
            continue;
        }
        else{
            if (!visited[a][i]) {
                if (s.do_have(i)) {
                    continue;
                }
                else {
                    if(i==b){
                        visited[a][i]=1;
                        visited[i][a]=1;
                        cout<<"get"<<endl;
                        if (i==NUM_POINTS-1) {
                            if (!s.is_empty()) {
                                for (int i=0; i<NUM_POINTS; i++) {
                                    visited[a][i]=0;
                                    show_v(visited);
                                }
                                dfs(s.pop(), b);
                            }
                            else return;
                        }
                        else continue;
                    }
                    else{
                        visited[a][i]=1;
                        visited[i][a]=1;
                        show_v(visited);
                        path.push(a);
                        s.push(a);
                        dfs(i, b);
                    }
                }
            }
            else {
                if(i==NUM_POINTS-1){
                    if (!s.is_empty()) {
                        for (int i=0; i<NUM_POINTS; i++) {
                            visited[a][i]=0;
                            show_v(visited);
                        }
                        dfs(s.pop(), b);
                    }
                    else return;
                }
                else continue;
            }
        }
    }
}

```
