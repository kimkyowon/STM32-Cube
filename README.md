# STM32Cube ide로 하는 공부!

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

## 06.08
