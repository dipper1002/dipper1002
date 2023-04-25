'''cpp

#include <iostream>
#define MOD 1000000007;
using namespace std;

int dp[801][1<<16];
int n, m;
int solve(int pos,int check)
{
    if(dp[pos][check]!=0)return dp[pos][check];
    int &ret = dp[pos][check];

    if(pos == 0)return ret = 1;

    int sum = 0;
    //if(((!(check&(1<<(m-1))))&&(!(check&(1<<(m+m-1))))) || ((check&(1<<(m-1)))&&(check&(1<<(m+m-1))))))


        if( (check&(1<<(m))) ) // 아래 불켜져있을때
        {
            int count = 0;
            if(pos%m!=0 && m!=1)if((check&(1<<(m-1))))count++;//오른쪽
            if(pos%m!=1 && m!=1)if((check&(1<<(m+1))))count++;//왼쪽
            //if(pos<=(n*m-m-m))
            if((check&(1)))count++;//아래

             if(pos/m!=0 || !(check&(1<<(m+m-1))) )//not last line + 마지막 라인 첫번째
            {
                if(count%2!=0)//짝수가 아니면
                {
                    sum += solve(pos-1,(check>>1)|(1<<(m+m-1)));//불킴
                }
                else //짝수면
                {
                    sum += solve(pos-1,(check>>1));//불끔
                }
            }
            else
            {
                int count2 = 0;
                if(m!=1 && m!=2  && pos%m != 0 && pos%m != m-1)if(check&1<<(m+m-2))count2++;//오른쪽
                //if(pos<=(n*m-m))
                if((check&(1<<(m-1))))count2++;//아래

                if(count%2!=0 && count2%2!=0)//짝수가 아니면
                {
                    sum += solve(pos-1,(check>>1)|(1<<(m+m-1)));//불킴
                }
                else if(count%2==0 && count2%2 == 0) //짝수면
                {
                    sum += solve(pos-1,(check>>1));//불끔
                }
                else
                {
                    return ret = 0;//불가능
                }
            }

        }
        else//아래 꺼져있으면?
        {
            if(pos/m!=0 || !(check&(1<<(m+m-1))) )
            {
            sum += solve(pos-1,(check>>1)|(1<<(m+m-1)));//불킴
            sum += solve(pos-1,(check>>1));//불끔
            }
            else
            {
                int count2 = 0;
                if(m!=1 && m!=2 && pos%m != 0 && pos%m != m-1)if(check&1<<(m+m-2))count2++;//오른쪽
                //if(pos<=(n*m-m))
                if((check&(1<<(m-1))))count2++;//아래

                if(count2%2!=0)//짝수가 아니면
                {
                    sum += solve(pos-1,(check>>1)|(1<<(m+m-1)));//불킴
                }
                else if(count2%2 == 0) //짝수면
                {
                    sum += solve(pos-1,(check>>1));//불끔
                }
            }
        }
    //cout<<pos<<" "<<sum<<endl;
    //else if()

    return ret = sum%MOD;
}
int main() {
    cin>>n>>m;
    cout<<solve(n*m,0);
	return 0;
}
  '''
