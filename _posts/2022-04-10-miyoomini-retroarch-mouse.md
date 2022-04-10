---
layout: posts
title:  "miyoo-mini retroarch 마우스 메세지 생성"
---

### 개요

dosbox 에서 마우스 커서를 움직이기 위해서 미유미니는 버튼 수가 부족합니다.
마우스 입력을 필요할 때 사용할 수 있도록 retroarch 코드 일부를 수정합니다.

### 구현 된 기능

- R2 입력시 토글로 마우스 기능을 on/off 합니다.
  마우스 기능 on 인 경우 dpad left/right/up/down 버튼은 마우스 커서를 이동시키고,
  A 버튼은 마우스 L 버튼으로, B버튼은 마우스 R 버튼으로 동작합니다.

### 소스 변경

> 소스 위치

    input/drivers/sdl_input.c

> 변수 선언

    static sdl_input_t sdlmouse;
    static int toggle_mouse=0;


> 입력 키 값을 마우스 값으로 변환

		switch(event.key.keysym.sym)
		{
			case SDLK_LEFT:
				if (event.type == SDL_KEYDOWN)
					sdlmouse.mouse_x = -2;
				else
					sdlmouse.mouse_x = 0;
			break;
			case SDLK_RIGHT:
				if (event.type == SDL_KEYDOWN)
					sdlmouse.mouse_x = 2;
				else
					sdlmouse.mouse_x = 0;
			break;
			case SDLK_UP:
				if (event.type == SDL_KEYDOWN)
					sdlmouse.mouse_y = -2;
				else
					sdlmouse.mouse_y = 0;
			break;
			case SDLK_DOWN:
				if (event.type == SDL_KEYDOWN)
					sdlmouse.mouse_y = 2;
				else
					sdlmouse.mouse_y = 0;
			break;
			case SDLK_SPACE: // Button A
				if (event.type == SDL_KEYDOWN)
					sdlmouse.mouse_l = 1;
				else
					sdlmouse.mouse_l = 0;
			break;
			case SDLK_LCTRL: // Button B
				if (event.type == SDL_KEYDOWN)
					sdlmouse.mouse_r = 1;
				else
					sdlmouse.mouse_r = 0;				
			break;
			case SDLK_BACKSPACE: // Button R2
				if (event.type == SDL_KEYDOWN) {

				}
				else {
					// toggle mouse
					if (toggle_mouse == 0)
						toggle_mouse = 1;
					else
						toggle_mouse = 0;
				}			
			break;
		}

> 마우스 모드일때(toggle_mouse) 마우스 값 전달

      case RETRO_DEVICE_MOUSE:
      case RARCH_DEVICE_MOUSE_SCREEN: 
         if(toggle_mouse == 1) 
         {
            switch (id)
            {

               case RETRO_DEVICE_ID_MOUSE_LEFT:
                  return sdlmouse.mouse_l; 
               case RETRO_DEVICE_ID_MOUSE_RIGHT:
                  return sdlmouse.mouse_r; 

               case RETRO_DEVICE_ID_MOUSE_X:
                  return sdlmouse.mouse_x;
               case RETRO_DEVICE_ID_MOUSE_Y:
                  return sdlmouse.mouse_y; 
            }
         }
         break;
        
