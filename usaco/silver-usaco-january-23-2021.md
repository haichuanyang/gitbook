# silver usaco january 23, 2021

```
//problem 1; 

#include <iostream>
#include <algorithm>
#include <vector>
#include <set>

using namespace std;

typedef long long LL;
typedef pair<int, int> PII;

const int N=100000;

int n, kk;

//vector<int> cows;
int cows[N];

//vector<PII> swp;
PII swp[N];

set<int> A[N]; //record possible positions for each cow

int main(){
    int tmp, a, b;

    cin>>n>>kk;

    for(int i=1; i<=kk; i++) {
        cin>>a>>b;
    //swp.push_back({a,b});
       swp[i].first=a;
        swp[i].second=b;
    }

    for (int j=1; j<=n; j++){
        A[j].insert(j);
        //cows.push_back(j);
        cows[j]=j;
        } // every cow has one position starting
    
    
    //simulate cow swapping for each swapping

    for (int j=0; j<n*kk; j++){

        for(int i=1; i<=kk; i++){

            //add cow position updates

            A[cows[swp[i].first]].insert(swp[i].second);
            A[cows[swp[i].second]].insert(swp[i].first);

            //update cows positions
            tmp = cows[swp[i].first];
            cows[swp[i].first] = cows[swp[i].second];
            cows[swp[i].second] = tmp;


        }

    }

    //size of each set element is the count of positions
    
    for (int i=1; i<=n; i++){cout<<A[i].size()<<endl;} 

    return 0;
}

//problem 3

#include <iostream>
#include <algorithm>
#include <vector>
#include <set>

using namespace std;

typedef long long LL;
typedef pair<int, int> PII;

const int N=1001;

int n, kk;

int a[N][N], c[N][N];
LL res;
int b[4];

int main(){

    int m1, m2, count;

    cin>>n;

    for (int i=1;i<=n; i++){
        for (int j=1;j<=n;j++){
        cin>>a[i][j];
        //cout<<a[i][j];
    }

    }


    //move through each 2x2 grid and pick 2 largest values

    for (int i=1; i<n; i++){

        for (int j=1;j<n;j++){
            //a[i][j], a[i+1][j], a[i][j+1], a[i+1][j+1]
           b[0]=a[i][j];
           b[1]=a[i+1][j];
           b[2]=a[i][j+1];
           b[3]=a[i+1][j+1];

           sort(b, b+4);
           cout<<b[3]<<' '<<b[2]<<endl;
           
            if(count<2 && (a[i][j]==b[2] || a[i][j]==b[3])) count++, c[i][j]=1;

            if(count<2 && (a[i+1][j]==b[2] || a[i+1][j]==b[3])) count++, c[i+1][j]=1;

            if(count<2 && (a[i][j+1]==b[2] || a[i][j+1]==b[3])) count++, c[i][j+1]=1;
            if(count<2 && (a[i+1][j+1]==b[2] || a[i+1][j+1]==b[3])) count++, c[i+1][j+1]=1;


           //res =res+b[3]+b[2];

           /*if(a[i+1][j]>a[i][j] && a[i][j+1]> a[i+1][j+1]){
               m1=a[i+1][j],m2=a[i][j+1];
               c[i+1][j]=1;c[i][j+1]=1;c[i][j]=0;c[i+1][j+1]=0;

             }  else {m1=a[i][j],m2=a[i+1][j+1];
                    c[i][j]=1;c[i+1][j+1]=1;  c[i][j+1]=0,c[i+1][j]=0;
             }

           if(a[i+1][j]>a[i][j+1] && a[i][j]> a[i+1][j+1]){
               m1=a[i+1][j],m2=a[i][j];
               c[i+1][j]=1; c[i][j]=1; c[i][j+1]=0;c[i+1][j+1]=0;
               }

           else {m1=a[i][j+1],m2=a[i+1][j+1]; 
            c[i][j+1]=1;c[i+1][j+1]=1;c[i][j]=0,c[i+1][j]=0;
           }


            if(a[i+1][j]>a[i+1][j+1] && a[i][j]> a[i][j+1]){m1=a[i+1][j],m2=a[i][j];
            c[i+1][j]=1,c[i][j]=1;c[i][j+1]=0,c[i+1][j+1]=0;
            }
           else {
               m1=a[i+1][j+1],m2=a[i][j+1];
                c[i+1][j+1]=1,c[i][j+1]=1;c[i][j]=0,c[i+1][j]=0;
           }
           */

        }
    }


    for (int i=1;i<=n;i++){
        for (int j=1;j<=n;j++){
            if(c[i][j]){res+=a[i][j];cout<<a[i][j]<<endl;}
        }
    }


    cout<<res<<endl;
    return 0;
}
```
