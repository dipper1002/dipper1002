# 네모왕국 온라인

- 개발일시
> (2019 ~ 2019)
- 개발 엔진
> Unity
- 개발 언어
> C#
- 개발 인원
> 본인 외 고등학교 친구 1명(2명)
- 담당 역할
> 게임 그래픽, 인게임 시스템 및 뒤끝을 이용한 아웃게임 보조, Photon 을 사용한 멀티플레이 구현
  
- 배경 소개
> 대학교 시절 취미로 계속 만들던 게임.
> 
> 첫 상업적 매출을 낸 게임이다.
>
> 처음으로 뒤끝이라는 네트워크 서비스를 활용하여 온라인 서버가 존재하는 게임을 개발하였음.
- 게임 소개
> 멀티 기반의 게임
>
> 랭킹을 이용한 싱글 스테이지 및 멀티 플레이에 랭킹을 부여하여 랭킹에 맞는 보상을 지급함
>
> 유닛 강화, 룬, 500스테이지까지 준비하여 여러 성장요소들을 준비했음

[유튜브 링크](https://youtu.be/ry5Y8NBa_4w)

[![Video Label](http://img.youtube.com/vi/ry5Y8NBa_4w/0.jpg)](https://youtu.be/ry5Y8NBa_4w)
![unnamed (3)](https://github.com/dipper1002/dipper1002/assets/42773970/c21fb4dd-cdc5-4fee-84c4-b2c4555dd537)
![unnamed (4)](https://github.com/dipper1002/dipper1002/assets/42773970/c670a806-51af-464e-a29b-9859fea1d07a)
![unnamed](https://github.com/dipper1002/dipper1002/assets/42773970/f0baa2ac-f3c0-492f-856b-dfccaab77e63)
![unnamed (1)](https://github.com/dipper1002/dipper1002/assets/42773970/8d1f26a5-73fa-4db5-b9c6-f96e040f7e0f)
![unnamed (2)](https://github.com/dipper1002/dipper1002/assets/42773970/0483ceb3-c5bb-4eff-9a5b-4eb20547261f)

## 주요 테크닉

### Photon (멀티 플레이)
### 뒤끝 (게임 서버)
### 비트마스킹

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

