cmake_minimum_required(VERSION 2.8.0)

# only test on linux
OPTION(ENABLE_PNG "Enable PNG support" 1)
OPTION(ENABLE_JPEG "Enable JPEG support" 1)
OPTION(ENABLE_TIFF "Enable TIFF support" 1)
OPTION(ENABLE_JXR "Enable JXR support" 1)

# currently these parts not working 
OPTION(ENABLE_WEBP "Enable WebP support" 0)
OPTION(ENABLE_RAW "Enable RAW support" 0)
OPTION(ENABLE_OPENJP "Enable OpenJPEG support" 0)
OPTION(ENABLE_OPENEXR "Enable OpenEXR support" 0)

project (FreeImage)

# Check parameters
INCLUDE(CheckIncludeFile)
INCLUDE(CheckIncludeFileCXX)
INCLUDE(CheckFunctionExists)
INCLUDE(CheckTypeSize)

add_definitions(-DNO_LCMS -D__ANSI__ -DDISABLE_PERF_MEASUREMENT)

# headers
include_directories ( Source Source/Metadata Source/FreeImageToolkit Source/LibJPEG Source/LibPNG Source/LibTIFF4 Source/ZLib Source/LibOpenJPEG Source/OpenEXR Source/OpenEXR/Half Source/OpenEXR/Iex Source/OpenEXR/IlmImf Source/OpenEXR/IlmThread Source/OpenEXR/Imath Source/OpenEXR/IexMath Source/LibRawLite Source/LibRawLite/dcraw Source/LibRawLite/internal Source/LibRawLite/libraw Source/LibRawLite/src Source/LibWebP Source/LibJXR Source/LibJXR/common/include Source/LibJXR/image/sys Source/LibJXR/jxrgluelib)

# SOURCES
set(Sources ${PROJECT_SOURCE_DIR}/Source)
file(GLOB DeprecationManager ${Sources}/DeprecationManager/*.c ${Sources}/DeprecationManager/*.cpp)
file(GLOB FreeImage ${Sources}/FreeImage/*.cpp ${Sources}/FreeImage/*.c)
file(GLOB FreeImageToolkit ${Sources}/FreeImageToolkit/*.c ${Sources}/FreeImageToolkit/*.cpp)
file(GLOB LibJPEG ${Sources}/LibJPEG/*.c ${Sources}/LibJPEG/*.cpp)
file(GLOB_RECURSE LibJXR ${Sources}/LibJXR/*.c ${Sources}/LibJXR/*.cpp)
file(GLOB LibOpenJPEG ${Sources}/LibOpenJPEG/*.cpp ${Sources}/LibOpenJPEG/*.c)
file(GLOB LibPNG ${Sources}/LibPNG/*.cpp ${Sources}/LibPNG/*.c)
file(GLOB_RECURSE LibRawLite ${Sources}/LibRawLite/*.c ${Sources}/LibRawLite/*.cpp)
file(GLOB LibTIFF4 ${Sources}/LibTIFF4/*cpp ${Sources}/LibTIFF4/*.c)
file(GLOB_RECURSE LibWebP ${Sources}/LibWebP/*.cpp ${Sources}/LibWebP/*.c)
file(GLOB Metadata ${Sources}/Metadata/*.c ${Sources}/Metadata/*.cpp)
file(GLOB_RECURSE OpenEXR ${Sources}/OpenEXR/*.cpp ${Sources}/OpenEXR/*.c)
file(GLOB ZLib ${Sources}/ZLib/*.cpp ${Sources}/ZLib/*.c)

# only works on linux
# remove compiling issues
list(REMOVE_ITEM LibJPEG ${Sources}/LibJPEG/jmemdos.c)
list(REMOVE_ITEM LibJPEG ${Sources}/LibJPEG/jmemmac.c)
list(REMOVE_ITEM LibJPEG ${Sources}/LibJPEG/ansi2knr.c)
list(REMOVE_ITEM LibTIFF4 ${Sources}/LibTIFF4/tif_unix.c)
list(REMOVE_ITEM LibTIFF4 ${Sources}/LibTIFF4/tif_vms.c)
list(REMOVE_ITEM LibTIFF4 ${Sources}/LibTIFF4/tif_win32.c)
list(REMOVE_ITEM LibTIFF4 ${Sources}/LibTIFF4/tif_wince.c)

SET(LIBS 
${DeprecationManager}
${FreeImage}
${FreeImageToolkit}
${ZLib}
)


IF(ENABLE_PNG)
  SET(LIBS ${LIBS} ${LibPNG})
ENDIF()
IF(ENABLE_JPEG)
  SET(LIBS ${LIBS} ${LibJPEG})
ENDIF()
IF(ENABLE_TIFF)
  SET(LIBS ${LIBS} ${LibTIFF4})
ENDIF()
IF(ENABLE_WEBP)
  SET(LIBS ${LIBS} ${LibWebP})
ENDIF()
IF(ENABLE_RAW)
  SET(LIBS ${LIBS} ${LibRawLite})
ENDIF()
IF(ENABLE_OPENJP)
  SET(LIBS ${LIBS} ${LibOpenJPEG})
ENDIF()
IF(ENABLE_OPENEXR)
  SET(LIBS ${LIBS} ${OpenEXR})
ENDIF()
IF(ENABLE_JXR)
  SET(LIBS ${LIBS} ${LibJXR})
ENDIF()


add_library ( freeimage ${LIBS} )

install ( TARGETS freeimage DESTINATION lib )
install ( FILES Source/FreeImage.h DESTINATION include )

