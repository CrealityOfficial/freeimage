__cc_find(png_static)
__assert_target(png_static)

__cc_find(open_jpeg)

set(FREE_IMAGE_HAS_TIFF 0)

set(freeImageRoot ${CMAKE_CURRENT_SOURCE_DIR})
set(freeImageSrcs FreeImage/Conversion.cpp
				  FreeImage/Conversion4.cpp
				  FreeImage/Conversion8.cpp
				  FreeImage/Conversion16_555.cpp
				  FreeImage/Conversion16_565.cpp
				  FreeImage/Conversion24.cpp
				  FreeImage/Conversion32.cpp
				  FreeImage/ConversionFloat.cpp
				  FreeImage/ConversionRGB16.cpp
				  FreeImage/ConversionRGBA16.cpp
				  FreeImage/ConversionRGBAF.cpp
				  FreeImage/ConversionRGBF.cpp
				  FreeImage/ConversionType.cpp
				  FreeImage/ConversionUINT16.cpp
				  FreeImage/Halftoning.cpp
				  FreeImage/tmoColorConvert.cpp
				  FreeImage/tmoDrago03.cpp
				  FreeImage/tmoFattal02.cpp
				  FreeImage/tmoReinhard05.cpp
				  FreeImage/ToneMapping.cpp
				  FreeImage/Conversion.cpp
				  Metadata/Exif.cpp
				  Metadata/FIRational.cpp
				  Metadata/FreeImageTag.cpp
				  Metadata/IPTC.cpp
				  Metadata/TagConversion.cpp
				  Metadata/TagLib.cpp
				  FreeImage/CacheFile.cpp
				  FreeImage/MultiPage.cpp
				  FreeImage/LFPQuantizer.cpp
				  FreeImage/NNQuantizer.cpp
				  FreeImage/WuQuantizer.cpp
				  FreeImage/BitmapAccess.cpp
				  FreeImage/ColorLookup.cpp
				  FreeImage/FreeImage.cpp
				  FreeImage/FreeImageIO.cpp
				  FreeImage/GetType.cpp
				  FreeImage/MemoryIO.cpp
				  FreeImage/PixelAccess.cpp
				  FreeImageToolkit/Background.cpp
				  FreeImageToolkit/BSplineRotate.cpp
				  FreeImageToolkit/Channels.cpp
				  FreeImageToolkit/ClassicRotate.cpp
				  FreeImageToolkit/Colors.cpp
				  FreeImageToolkit/CopyPaste.cpp
				  FreeImageToolkit/Display.cpp
				  FreeImageToolkit/Flip.cpp
				  FreeImageToolkit/MultigridPoissonSolver.cpp
				  FreeImageToolkit/Rescale.cpp
				  FreeImageToolkit/Resize.cpp
				  FreeImage/Plugin.cpp
				  FreeImage/PluginBMP.cpp
				  FreeImage/PluginICO.cpp
				  )

set(LIBS)
set(DEFS _CRT_SECURE_NO_DEPRECATE)		

if(TARGET open_jpeg)
	set(FREE_IMAGE_HAS_JPEG 1)
endif()
		  
if(FREE_IMAGE_HAS_JPEG)
	add_definitions(-DFREE_IMAGE_HAS_JPEG_ENBALE)
	list(APPEND freeImageSrcs FreeImageToolkit/JPEGTransform.cpp)
	list(APPEND freeImageSrcs FreeImage/PluginJPEG.cpp)
	list(APPEND LIBS open_jpeg)
endif()

if(FREE_IMAGE_HAS_TIFF)
	list(APPEND freeImageSrcs Metadata/XTIFF.cpp)
endif()

if(TARGET png)
	set(FREE_IMAGE_HAS_PNG 1)
endif()

if(FREE_IMAGE_HAS_PNG)
	add_definitions(-DFREE_IMAGE_HAS_PNG_ENBALE)
	list(APPEND freeImageSrcs FreeImage/ZLibInterface.cpp)
	list(APPEND freeImageSrcs FreeImage/PluginPNG.cpp)
	list(APPEND LIBS png_static)
endif()

if(ANDROID)
	find_library(log-lib log)
	list(APPEND LIBS ${log-lib})
endif()

__add_real_target(freeimage_static lib SOURCE ${freeImageSrcs}
								INC ${freeImageRoot}
								ILIB ${LIBS}
								DEF ${DEFS} FREEIMAGE_LIB
								INTERFACE_DEF FREEIMAGE_LIB
								INTERFACE ${freeImageRoot}
								)

if(CC_INSTALL_FREEIMAGE_STATIC)
	__install_directory_specif(${CMAKE_CURRENT_SOURCE_DIR}/ freeimage)
endif()	
