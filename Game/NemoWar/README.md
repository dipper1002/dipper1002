# 네모왕국

- 개발일시
> (2022 ~ ) 온라인 서비스중
- 개발 엔진
> Unity
- 개발 언어
> C#
- 개발 인원
> 본인 외 고등학교 친구 2명(3명)
- 담당 역할
> 게임 그래픽, 인게임 시스템 및 뒤끝을 이용한 아웃게임 보조, 뒤끝매치 을 사용한 멀티플레이 구현
  
- 배경 소개
> 대학교 시절 취미로 계속 만들던 게임.
> 
> 저번 게임의 전략적 깊이가 너무 얉았다는 생각이 들어, 전략적인 요소를 대폭 강화하고자 함.
>
> UI 시스템등 이전게임에 비해 많이 좋아졌으나 BM에 대한 깊이있는 고민 부재
>
> 저번 네모왕국 온라인 당시, 해킹툴로 많이 뚫리는 것을 경험한 뒤 안티치트 에셋을 활용하여 데이터를 숨기고 데이터 조작이 감지 될 경우 서버에 로그를 남기도록 함
- 게임 소개
> 멀티 기반의 게임
>
> 랭킹을 이용한 싱글 스테이지 및 멀티 플레이에 랭킹을 부여하여 랭킹에 맞는 보상을 지급함
>
> 특히 멀티플레이의 초점을 강하게 집중하여 멀티플레이를 통해 지속적인 유저간의 경쟁을 유도하였음.

[구글플레이스토어 링크](https://play.google.com/store/apps/details?id=com.dippergames.nemokingdom&hl=ko)

[유튜브 링크](https://youtu.be/kRmTY_UlJ3k)

[![Video Label](http://img.youtube.com/vi/kRmTY_UlJ3k/0.jpg)](https://youtu.be/kRmTY_UlJ3k)

![unnamed (5)](https://github.com/dipper1002/dipper1002/assets/42773970/e72023cd-1b96-4295-ab68-80fdf64ddcae)
![unnamed (6)](https://github.com/dipper1002/dipper1002/assets/42773970/d1dbe153-f6b5-4754-b583-1c3c81363fbd)
![unnamed (7)](https://github.com/dipper1002/dipper1002/assets/42773970/cf365192-c03f-4f1f-98af-d8bfa5c4fd2e)
![unnamed (8)](https://github.com/dipper1002/dipper1002/assets/42773970/a90bc0ff-b52b-45b3-92c9-bf0034270cf1)

## 주요 테크닉

### 뒤끝 (게임 서버 & 실시간 멀티 플레이)
### 비트마스킹을 이용한 멀티 플레이 전송 데이터 감축
### 안티치트를 이용한 데이터 암호화, 데이터 변경 감지 및 서버로그 보관

멀티플레이 코드의 일부

당시, 200대 200의 많은 유닛의 데이터를 보내야하는 멀티 플레이 환경에서, 전송량을 최대한 압축하기위해
비트마스킹 테크닉을 이용해 최대한 전송데이터를 줄였던것을 코드에서 확인할 수 있다.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using BackEnd;
using BackEnd.Tcp;
using UserData;
using UnityEngine.SceneManagement;
using UnityEngine.UI;
public class s_ingame_data_manager : MonoBehaviour
{
    [SerializeField]
    s_namo_manager namo_manager;
    [SerializeField]
    Object main_scene;

    [SerializeField]
    Text time_text;
    [SerializeField]
    GameObject loading;

    [SerializeField]
    Text T_white_nick;
    [SerializeField]
    Text T_black_nick;

    [SerializeField]
    Text T_white_rating;
    [SerializeField]
    Text T_black_rating;

    [SerializeField]
    Animator intro_ani;

    private bool other_player_on;

    public int[] other_player_deck;
    private int other_rating;

    private bool online;
    private int total_count;

    private bool result;
    private bool start;
    public void SummonUnit(bool white, int a, int num, int summon)
    {
        if (white)
        {
            byte[] b = new byte[4];
            b[0] = 0;
            b[1] = (byte)(a / 256);
            b[2] = (byte)(a % 256);
            b[3] = (byte)(num);
            if (other_player_on)
                Backend.Match.SendDataToInGameRoom(b);
        }
        else
        {
            byte[] b = new byte[5];
            b[0] = 1;
            b[1] = (byte)(a / 256);
            b[2] = (byte)(a % 256);
            b[3] = (byte)(num);
            b[4] = (byte)(summon);
            if (other_player_on)
                Backend.Match.SendDataToInGameRoom(b);
        }
    }
    public void SummonUnit(bool white, int a, int num, float x ,float y, int summon)
    {
        if (white)
        {
            byte[] b = new byte[9];
            b[0] = 7;
            b[1] = (byte)(a / 256);
            b[2] = (byte)(a % 256);
            b[3] = (byte)(num);
            b[4] = (byte)((int)(x*1000) / 256);
            b[5] = (byte)((int)(x * 1000) % 256);
            b[6] = (byte)((int)(y * 1000) / 256);
            b[7] = (byte)((int)(y * 1000) % 256);
            b[8] = (byte)summon;
            if (other_player_on)
                Backend.Match.SendDataToInGameRoom(b);
        }
        else
        {
            byte[] b = new byte[9];
            b[0] = 8;
            b[1] = (byte)(a / 256);
            b[2] = (byte)(a % 256);
            b[3] = (byte)(num);
            b[4] = (byte)((int)(x * 1000) / 256);
            b[5] = (byte)((int)(x * 1000) % 256);
            b[6] = (byte)((int)(y * 1000) / 256);
            b[7] = (byte)((int)(y * 1000) % 256);
            b[8] = (byte)summon;
            if (other_player_on)
                Backend.Match.SendDataToInGameRoom(b);
        }
    }

    ...

}

```
