# STM32Cube ide로 하는 공부!

## < 06.05 >

### 1) blinky

```c
while(1){
  HAL_GPIO_WritePin(설정 핀 포트, 핀 이름, GPIO_PIN_RESET);  //led 핀 켜기
  delay(원하는 시간);
  
  HAL_GPIO_WritePin(설정 핀 포트, 핀 이름, GPIO_PIN_SET);  //led 핀 끄기
  delay(원하는 시간);
  
}
```

### 2) blinky GPIO output control with ODR register
```c
while(1){
  GPIO ->ODR = 0xf000 // 1111 0000 0000 0000 (led 4개가 켜짐)
  HAL_Delay(1000);
  
  GPIO ->ODR = 0xe000 // 1110 0000 0000 0000 (led 3개가 켜짐)
  HAL_Delay(1000);
  
  GPIO ->ODR = 0xc000 // 1111 0000 0000 0000 (led 2개가 켜짐)
  HAL_Delay(1000);
  
  GPIO ->ODR = 0x8000 // 1100 0000 0000 0000 (led 1개가 켜짐)
  HAL_Delay(1000);
  
  GPIO ->ODR = 0x0000 // 0000 0000 0000 0000 (led 다 꺼짐)
  HAL_Delay(1000);
}
```

## < 06.08 >
STM32의 타이머는 Basic , General purpose, advanced timer로 구분하는데 이때 이 타이머는 
내부 clock과 외부 clock으로 구동될 수 있습니다.

이번에 사용한 것은 General purpose로 타이머의 기능은 모두 제공되고 모터 제어나 Digital Power Conversion에 
적합한 Complementary signal, Dead time insertion, Emergency shut-down input등의 기능을 제공합니다.

## 타이머 정보
![image](https://user-images.githubusercontent.com/50939918/172609690-9c44444e-012b-48ac-8851-f35b37434655.png)
이러한 타이머들이 어떤 버스에 연결되어 있는지에 따라 Internal Clock 소스의 Freq가 결정되므로 확인이 필수적입니다!

### 타이머 이벤트가 발생하는 주기 계산법
![image](https://user-images.githubusercontent.com/50939918/172610277-2e1a886e-8f12-45ad-8b04-10ec3712b6f4.png)

저는 14번 핀으로 해서 설정을 보니 이렇게 되어 있었습니다.
```c
TIM_HandleTypeDef htim14 
```

1. Poling 방식

```c
while(1){
  if(__HAL_TIM_GET_FLAG(&htim14, TIM_FLAG_UPDATE)){ //현재 update가 되었는지 확인
    __HAL_TIM_CLEAR_IT(&htim14, TIM_IT_UPDATE); //Clear해주고
    HAL_GPIO_TogglePin(LD3_GPIO_Port,LD3_Pin); //led켜짐
    }
  }
}
```

2. Interrupt 방식

```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim){ //Callback함수 생성
   if(htim->Instance==TIM14){
      HAL_GPIO_TogglePin(LD3_GPIO_Port, LD3_Pin);
   }
}
```
작성 후 메인함수에 아래 코드를 넣어 실행

```c
HAL_TIM_Base_Start_IT(&htim14); 
```
