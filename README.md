# CSE306-Project

#include <iostream>
#include <fstream>
#include <stack>
using namespace std;

void runDFS(int vertex, int **graph, int source, int *visited)
{
       stack <int> dfsStack;
       visited[source] = 1;
       dfsStack.push(source);
       while (!dfsStack.empty())
       {
           int top = dfsStack.top();
           dfsStack.pop();
           for (int i = 0; i <vertex; i++)
           {
               if(graph[top][i] == 1)
               {
                   if(visited[i] == 0)
                   {
                       dfsStack.push(i);
                       visited[i] = 1;
                   }
                }
            }
            visited[top] = 2;
        }
}


int printConnectedComponent(int vertex, int *visited){
    int count=0;
    for(int i=0; i<vertex; i++)
    {
        if(visited[i]==2)
        {
           cout<<"-"<<i;
           visited[i]=3;
           count++;
        }
    }
    cout<<endl<<"Number of people in a group: "<<count<< endl<<endl;
    return count;
}

int main()
{
    int max;
    ifstream fileinput;
    fileinput.open("input.txt");
    for(int i=0; i<2; i++)
    {
       int  count=0;
       int n1,n2, N, M, a[10];
       cout<<"               Test Case "<<i+1<<"         "<<endl;
       cout<<"               ----------- "<<endl;
       fileinput>>N>>M;
       int **graph;
       graph=new int*[N];
       for(int i=0; i<N; i++)
      {
        graph[i]=new int[N];
      }

      for(int i=0; i<M; i++)
      {
        fileinput>>n1>>n2;
        graph[n1-1][n2-1]=1;
        graph[n2-1][n1-1]=1;
      }

     int *visited=new int[N];

     for(int i=0; i<N; i++)
     {
        if(visited[i]==0)
        {
           runDFS(N,graph,i,visited);
           cout<<"Mutual friends Found: ";
           a[count]=printConnectedComponent(N, visited);
           count++;
        }
     }
     if(count>=2)
     {
         max=a[0];
         for(int j=0; j<count-1; j++)
         {
             if(a[j+1]>max)
             {
                max=a[j+1];
             }
             else
             {
                continue;
             }
         }
         cout<<"Number of different mutual friends groups: "<<count<<endl;
         cout<<"Number of largest group of people: "<<max<<endl<<endl;
     }
     else
     {
        cout<<"Number of different mutual friends groups: "<<count<<endl;
        cout<<"Number of largest group of people: "<<a[0]<<endl<<endl;
     }
    }
    return 0;
}
