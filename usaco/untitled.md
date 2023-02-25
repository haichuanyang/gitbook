# USACO 2021 January Contest, Bronze Problem

### &#x20;USACO 2021 January Contest, Bronze Problem 2. Even More Odd Photos

missed cases 4 7 11 - code logic too complicated, sure to fail

```
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

long long N, ans;
vector<int> a;
long long b;
long long neven, nodd;

int cven(int x, int y){
    int cnt=0;
    if(y==0) return 1;
    if(x==0) { if(y%3==0){ cnt=cnt+y/3*2;}
    if(y%3==1){cnt=cnt+(y-3)/2+1;}
    if(y%3==2){cnt=cnt+y/2+1;}
    return cnt;
    }
    while(x>0 && y>0){
        x--;cnt++;y--;cnt++;
    }
    if(x>0){cnt++;return cnt;}
    if(y%3==0){ cnt=cnt+y/3*2;}
    if(y%3==1){cnt=cnt+(y-3)/2+1;}
    if(y%3==2){cnt=cnt+y/2+1;}
    return cnt;
}

int main(){
    cin>>N; //cout<<N;
    for(int i=0;i<N;i++){
        cin>>b;
        //cout<<b;
        if(b % 2 ==0 )neven++;
        else if(b%2 !=0) nodd++;
    }
    cout<<neven<<' ' <<nodd<<endl;

   
    cout<<cven(neven, nodd)<<endl;
    return 0;

}
```



```
In the best case, each cow is in its own group, and the answer is N. When is this allowed to happen?

Let O be the number of odd cows and E be the number of even cows initially in the list. For each cow to be in its own group, either E=O or E=O+1.

If this is not the case, then we must have groups with more than one cow. There are two cases.

If E>O+1, then the problem is that we have too many even cows. However, note that two even numbers added together is an even number. Therefore, we can do the assignment as follows - take one even cow, then one odd cow, and repeat this O times. The remaining cows are all even, and we can put them all in the same group. In this case, the answer is 2O+1.

The other case is when E<O, so we have too many odd cows. Note that two odd numbers added together is an even number. To deal with having too many odd cows, we have to take two odd cows and pair them, which is equivalent to having two fewer odd cows and one more even cow. We can repeat this process of pairing two odd cows until E≥O, and then we can use the above logic to compute the answer.

Here is Brian Dean's code.

#include <iostream>
using namespace std;
 
int main(void)
{
  int O=0, E=0, N, x;
  cin >> N;
  for (int i=0; i<N; i++) {
    cin >> x;
    if (x % 2 == 0) E++; else O++;
  }
  while (O > E) { O=O-2; E++; }
  if (E > O+1) E = O+1;
  cout << E + O << "\n";
}
```

### USACO 2021 January Contest, Bronze Problem 3. Just Stalling

```
#include <algorithm>
#include <iostream>
#include <vector>

typedef long long LL;

using namespace std;

const int N=22;
LL ans, n,tmp;
//vector<int> ab;
int a[N],b[N],ab[N];
LL cnt=0, facto=1;

LL fact(int n){
    LL re=1;
    for (int i=1;i<=n;i++)re=re*i;
    return re;
}


int main(){

    cin>>n;
    for (int i=0;i<n;i++) cin>>a[i];
    for(int i=0;i<n;i++){
        cin>>b[i];}

sort(a, a+n);
sort(b, b+n);

cnt=0;

for (int i=n-1; i>0; i--){
    for(int j=n-1;j>=0;j--){
        if(a[i]<=b[j]){tmp++;}
    }//cout<<tmp<<endl;
    ab[i]=tmp-cnt; cnt++;tmp=0;
    //cout<<ab[i]<<' '<<cnt<<endl;
}

ans=1;

for (int i=1;i<n;i++) ans*=ab[i];
    cout<<ans<<endl;
    return 0;



}
```



### USACO 2021 January Contest, Bronze Problem 1. Uddered but not Herd



```
#include <algorithm>
#include <iostream>

using namespace std;

#define N 26

char s1[26];
string s2;
int order[26];
int cnt;

bool front(char a, char b){
    int af, bf;
    for (int i=0; i<26;i++){
        if(s1[i]==a) af=i;
        if(s1[i]==b) bf=i;
    }
    //cout<<a<<b<<endl;
    if(af<=bf)return true;
    else return false;
}

int main(){
    cin>>s1;
    cin>>s2;

    for (int i=0; i<s2.size()-1; i++){
        if(front(s2[i+1],s2[i])) cnt++;
    }
    cout<<cnt+1<<endl;
}
```
