# STM32의 예제 실습

### led on/off

'''c
while(1){
  HAL_GPIO_WritePin(설정 핀 포트, 핀 이름, GPIO_PIN_RESET);  //led 핀 켜기
  delay(원하는 시간);
  
  HAL_GPIO_WritePin(설정 핀 포트, 핀 이름, GPIO_PIN_SET);  //led 핀 끄기
  delay(원하는 시간);
  
}
