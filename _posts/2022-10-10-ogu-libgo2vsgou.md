---
layout: posts
title:  "ogu libgo2 코드 libgou로 수정"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,lbgou]
---

libgo2 로 작성된 코드를 libgou로 대체하기.
retrorun go2와 gou 코드를 비교하여 기존에 작성된 코드를 수정한다.

video.cpp 기준

libgo2 | libgou
------ | --------
#include <go2/display.h> | #include <gou/display.h>
                        |  #include <gou/context3d.h>
go2_display_t* display; | gou_display_t* display;
go2_surface_t* surface; | gou_surface_t* surface;
go2_surface_t* display_surface; | gou_surface_t* display_surface;
go2_context_t* context3D; | gou_context3d_t* context3D;
go2_context_attributes_t attr; | gou_context3d_attributes_t attr;
go2_context_create | gou_context3d_create
go2_context_make_current | gou_context3d_make_current
go2_surface_create | gou_surface_create
return fbo; | return gou_context3d_fbo_get(context3D);
w = go2_display_height_get(display); | w = gou_display_width_get(display);
h = go2_display_width_get(display); | h = gou_display_height_get(display);
go2_surface_map | gou_surface_map
go2_drm_format_get_bpp | gou_drm_format_get_bpp
go2_surface_format_get | gou_surface_format_get
go2_surface_stride_get | gou_surface_stride_get
go2_presenter_post | gou_display_present


go2_frame_buffer_t* frame_buffer;
go2_presenter_t* presenter;




display = go2_display_create();
presenter = go2_presenter_create(display, DRM_FORMAT_RGB565, 0xff080808);  // ABGR

-->

display = gou_display_create();
gou_display_background_color_set(display, 0xff080808);  // ABGR

audio.cpp 기준

libgo2 | libgou
------ | ------
#include <go2/audio.h> | #include <gou/audio.h>
static go2_audio_t* audio; | static gou_audio_t* audio;
audio = go2_audio_create(freq); | audio = gou_audio_create(freq);
go2_audio_volume_set | gou_audio_volume_set
go2_audio_volume_get | gou_audio_volume_get
go2_audio_submit | gou_audio_submit

input.cpp 기준

libgo2 | libgou
------ | ------
#include <go2/input.h> | #include <gou/input.h>




premake4.lua / Makefile  차이

LDFLAGS   +=  -lgo2 -lrga

->

LDFLAGS   +=  -lgou
