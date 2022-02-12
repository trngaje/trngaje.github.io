---
layout: posts
title:  "SDL1.2 를 SDL2.0으로 변경(migration) 방법"
---

### 변경 방법 개요

SDL 함수는 디스플레이 사운드 키보드 조이스틱을 간편하게 사용하기 위한 함수입니다.
Dingux 류에서는 대부분 SDL1.2 로 작성되어 있고 OGA 류에서는 SDL2.0으로 작성되어 있습니다.
SDL1.2 로 작성된 유틸이나 홈브류 게임을 SDL2.0 라이브러리를 사용하도록 변경하는 방법은
아래 순서로 진행하게 됩니다. 한번 관련 함수에 익숙해 지면 다음 작업시 참고가 되기 때문에
시간을 갖고 지금까지 작업된 소스를 보고 내용을 정리합니다.

- 빌드 환경 변경
- 사용하는 헤더 변경
- 비슷한 함수로 변경

### 빌드 환경 변경

SDL1.2

    CC_OPTS		= -O2 -fomit-frame-pointer -fdata-sections -ffunction-sections $(F_OPTS)
    CFLAGS		= -I$(SDL_INCLUDE) $(CC_OPTS)
    LDFLAGS     = -lSDLmain -lSDL -lSDL_mixer -lSDL_image -lSDL_gfx -lm -lstdc++ -Wl,--as-needed -Wl,--gc-sections -flto

SDL2.0

    CC_OPTS		= -O2 -fomit-frame-pointer -fdata-sections -ffunction-sections $(F_OPTS) -DOGS_SDL2
    CFLAGS		= -I$(SDL_INCLUDE) $(CC_OPTS) `sdl2-config --cflags`
    LDFLAGS     = `sdl2-config --libs` -lSDL2 -lSDL2_mixer -lSDL2_image -lSDL2_gfx -lm -lstdc++ -Wl,--as-needed -Wl,--gc-sections -flto


### 사용하는 헤더 변경




SDL1.2

    #include <SDL/SDL.h>
    #include <SDL/SDL_mixer.h>
    #include <SDL/SDL_image.h>
    #include <SDL/SDL_rotozoom.h>
    #include <SDL/SDL_gfxPrimitives.h>

SDL2.0

    #include <SDL2/SDL.h>
    #include <SDL2/SDL_mixer.h>
    #include <SDL2/SDL_image.h>
    #include <SDL2/SDL2_rotozoom.h>
    #include <SDL2/SDL2_gfxPrimitives.h>

### 비디오 초기 설정 함수 변경

SDL1.2

    return SDL_SetVideoMode(320, 240, 16, SDL_SWSURFACE);

SDL2.0

    SDL_Window* sdlWindow=NULL;
    SDL_Surface* sdlSurface=NULL;
    int lcd_width, lcd_height;

    SDL_DisplayMode DM;
    SDL_GetCurrentDisplayMode(0, &DM);
    lcd_width = DM.w;
    lcd_height = DM.h;

    sdlWindow = SDL_CreateWindow("Zelda Picross",
                              SDL_WINDOWPOS_UNDEFINED,  
                              SDL_WINDOWPOS_UNDEFINED,  
                              lcd_width, lcd_height,
                              SDL_WINDOW_OPENGL); 
    sdlSurface = SDL_GetWindowSurface(sdlWindow);
	return SDL_CreateRGBSurface(SDL_SWSURFACE, lcd_width, lcd_height, 32, 0, 0, 0, 0);

### 비디오 화면 업데이트 함수 변경

SDL1.2

    SDL_Flip(gpScreen);

SDL2.0

    SDL_BlitScaled(gpScreen, &src, sdlSurface, &dst);
    SDL_UpdateWindowSurface(sdlWindow);

### 비슷한 함수로 변경

SDL1.2

    SDL_WarpMouse(xmouse, ymouse);
    SDL_Surface* im = SDL_DisplayFormat(tmp);
    SDL_SRCCOLORKEY

SDL2.0

    extern SDL_Window* sdlWindow;
    SDL_WarpMouseInWindow(sdlWindow, xmouse, ymouse);
    SDL_Surface* im = SDL_ConvertSurfaceFormat(tmp, SDL_GetWindowPixelFormat(sdlWindow), 0);
    SDL_TRUE

키보드 키 값 변경


SDL1.2

    #define CONFIRM_BUTTON SDLK_LCTRL
    Uint8* keys = SDL_GetKeyState(NULL);
    if (keys[SDLK_UP]) {
    if (keys[SDLK_DOWN]) {

SDL2.0

    #define CONFIRM_BUTTON SDL_SCANCODE_LCTRL
    Uint8* keys = (Uint8*)SDL_GetKeyboardState(NULL);
    if (keys[SDL_SCANCODE_UP]) {
    if (keys[SDL_SCANCODE_DOWN]) {
