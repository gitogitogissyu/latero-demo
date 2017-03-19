include(FindPkgConfig)

# add /usr/loca/lib/pkgconfig (necessary on macOS)
set( ENV{PKG_CONFIG_PATH} "$ENV{PKG_CONFIG_PATH}:/usr/local/lib/pkgconfig" )

pkg_check_modules (LATERO_GRAPHICS latero-graphics)
link_directories(${LATERO_GRAPHICS_LIBRARY_DIRS})
include_directories(${LATERO_GRAPHICS_INCLUDE_DIRS})

set(studio_SRCS
    mainwindow.h
    mainwindow.cpp
    main.cpp
    managerwidget.h
    managerwidget.cpp
    drawing.h
    drawing.cpp)

add_executable(latero-studio ${studio_SRCS})

#dependent on OS
if(APPLE)
target_link_libraries(latero-studio "-framework Foundation" ${LATERO_GRAPHICS_LIBRARIES} boost_program_options-mt)
else(APPLE)
target_link_libraries(latero-studio  ${LATERO_GRAPHICS_LIBRARIES} boost_program_options audiere)
endif(APPLE)

install_programs(/bin latero-studio)
