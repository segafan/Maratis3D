## Find source and headers

FILE(GLOB SOURCES
  src/clipboard.c
  src/context.c
  src/gamma.c
  src/init.c
  src/input.c
  src/joystick.c
  src/monitor.c
  src/time.c
  src/window.c
)

IF(APPLE)
  FILE(GLOB OS_HEADERS src/cocoa_*.h src/nsgl_*.h)
  FILE(GLOB OS_SOURCES src/cocoa_*.c src/nsgl_*.c src/cocoa_*.m src/nsgl_*.m)
  ADD_DEFINITIONS(-D_GLFW_COCOA)
  ADD_DEFINITIONS(-D_GLFW_NSGL)
ENDIF(APPLE)

IF(UNIX AND NOT APPLE)
  FILE(GLOB OS_HEADERS src/x11_*.h src/glx_*.h)
  FILE(GLOB OS_SOURCES src/x11_*.c src/glx_*.c)
  ADD_DEFINITIONS(-D_GLFW_X11)
  ADD_DEFINITIONS(-D_GLFW_GLX)
  ADD_DEFINITIONS(-D_GLFW_HAS_GLXGETPROCADDRESS)
ENDIF(UNIX AND NOT APPLE)

IF(CMAKE_HAS_SORT)
  LIST(SORT SOURCES)
  LIST(SORT OS_HEADERS)
  LIST(SORT OS_SOURCES)
ENDIF(CMAKE_HAS_SORT)

## Build
ADD_DEFINITIONS(-D_GLFW_USE_OPENGL)
ADD_DEFINITIONS(-D_GLFW_USE_MENUBAR)
ADD_DEFINITIONS(-D_GLFW_USE_CHDIR)

INCLUDE_DIRECTORIES(
  include
)

ADD_LIBRARY(glfw ${SOURCES} ${OS_HEADERS} ${OS_SOURCES})
