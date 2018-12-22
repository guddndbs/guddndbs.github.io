---
layout: post
cover: assets/images/note.jpg
title: EV3로 만든 THAAD의 제작과정과 코드
---
10월 24일
처음으로 조원들과 만나 아이디어회의를 진행했다.
다양한 아이디어가 나왔고 우리조원들 모두 운동을 좋아했기에
운동기구에 대한 아이디어가 많이 나왔고
처음 나온 아이디어는 배구선수들이 스스로 훈련할 수 있도록 만들어진
자동 토스 기계였다.
배구공을 다루기에는 현실적으로 힘들어서 우리는 탁구공을 발사하는 기구를 만들기로 했다.
이름도 THAAD로 나름 중의적인 의미를 가지는 이름으로 지었다.

11월 22일
우선 로봇을 설계해보기 시작했다.
예상했던것보다 모터가 훨씬 무게가 나갔고 이로인해 회전할 때 관성에 의해 로봇이 쉽게 움직였다. 이를 막기 위해 무게중심을 아래쪽으로 두고 연결부분의 레고에 힘을 주었다.

11월 29일
설계한 로봇을 바탕으로 코드를 짜기 시작했다.
라인트레이싱은 수업시간에 배운 부분이라 크게 어려움이 없었지만
미디움 모터를 부착한 초음파센서를 제어하는데 어려움이 컸다.

12월 9일
미디움 모터를 제어하는데 어려움을 느껴 결국 초음파센서를 회전시키는것을 포기했다. 하지만 회전을 하는 부분은 꼭 필요하므로 라인트레이싱을 목적으로 설계했던 바퀴를 물체를 탐지하기위해 회전하는 터렛으로 바꾸었다.

12월 11일
완성된 로봇으로 작동시키기 위해 코드를 수정했다.
라인트레이싱과 미디움 모터를 제외하니 코드가 훨씬 가벼워졌다.

12월 13일
최종발표를 마쳤다.
지금까지 해왔던 오픈소스 과목에 비하면 제일 무난했던 제작과정이었지만
우리가 처음 계획했던대로 만들지 못해 아쉬움이 컸다.
발표 후반에 교수님이 피드백을 해주시면서 아이디어를 주셨고 그 부분을 미리 인지하지 못한것도 아쉬웠다.


코드

#pragma config(Motor, motorB, lm, tmotorEV3_Large, PIDControl, encoder)
#pragma config(Motor, motorC, rm, tmotorEV3_Large, PIDControl, encoder)
#pragma config(Motor, motorD, sm, tmotorEV3_Large, PIDControl, encoder)
#pragma config(Motor, motorA, fm, tmotorEV3_Large, PIDControl, encoder)
#pragma config(Sensor, S4, ss, SensorEV3_Ultrasonic)

int convert(float dist){
   return (int)(360.0 * dist / 17.58);
}

task main()
{
   int ditect = 256;
   int enc_degree;
   int count = 0;
   enc_degree = convert(5.0);
   
   int save = 0;
   
   while(true){
        save = 0;
        count = 0;
      bool check = false;
         setMotorSpeed(rm, 30);
      setMotorSpeed(lm, 30);
      
      sleep(3000);
      
      setMotorSpeed(rm, 0);
      setMotorSpeed(lm, 0);
      ditect = getUSDistance(ss);
      
      for(int i = 0 && check != true; i < 20; i++){
         if(ditect < 10){
            save = i;
            check = true;
            count = 1;
            break;
         }
         setMotorSpeed(sm, 3);
         sleep(100);
         ditect = getUSDistance(ss);
      }
         setMotorSpeed(sm,0);
         for(int i = 0 && check != true; i < 20; i++){
        if(ditect < 10){
           save = i;
           check = true;
           count = 2;
           break;
         }
         setMotorSpeed(sm, -3);
         sleep(200);
         ditect = getUSDistance(ss);
      }
      for(int i = 0;i<20;i++){
         setMotorSpeed(sm,3);
         sleep(100);
      }
      
         setMotorSpeed(sm,0);
      if(check == true){
            resetMotorEncoder(fm);

            while(getMotorEncoder(fm) < enc_degree){
               setMotorSpeed(fm, 100);
            }

            setMotorSpeed(fm, 0);

            sleep(1000);

            while(getMotorEncoder(fm) > 0){
               setMotorSpeed(fm, -10);
            }

            setMotorSpeed(fm, 0);

            sleep(1000);
            resetMotorEncoder(sm);
            /*if(count == 1){
               for(int i = 0;i<save+1;i++){
                     //setMotorSpeed(sm,-30);
                     resetMotorEncoder(sm);
                  sleep(200);
                  }
            }
            if(count == 2){
               for(int i = 0;i<save+1;i++){
                     //setMotorSpeed(sm,30);
                     resetMotorEncoder(sm);
                  sleep(200);
                  }
               }*/
             }
      
      /*for(int i = 0 && check == true; i < 20; i++){
         if(ditect < 10){
            save = i;
            check = true;
            break;
         }
         setMotorSpeed(sm, -5);
         sleep(100);
         ditect = getUSDistance(ss);
      }
         setMotorSpeed(sm,0);
      if(check == true){
            resetMotorEncoder(fm);

            while(getMotorEncoder(fm) < enc_degree){
               setMotorSpeed(fm, 100);
            }

            setMotorSpeed(fm, 0);

            sleep(1000);

            while(getMotorEncoder(fm) > 0){
               setMotorSpeed(fm, -10);
            }

            setMotorSpeed(fm, 0);

            sleep(3000);
            
            setMotorSpeed(sm, 5);
            sleep(100*save);
      }*/
      setMotorSpeed(rm, 30);
        setMotorSpeed(lm, -30);
      setMotorSpeed(sm,0);
        sleep(1450);
      
/*      int dist = getUSDistance(ss);

      if(dist < 30){
            resetMotorEncoder(fm);

            while(getMotorEncoder(fm) < enc_degree){
               setMotorSpeed(fm, 100);
            }

            setMotorSpeed(fm, 0);

            sleep(1000);

            while(getMotorEncoder(fm) > 0){
               setMotorSpeed(fm, -10);
            }

            setMotorSpeed(fm, 0);

            sleep(3000);
      }*/

   }

}

