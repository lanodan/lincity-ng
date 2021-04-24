cmake_minimum_required(VERSION 3.20)
project (LinCity-NG VERSION 2.9
                    DESCRIPTION "City Simulation Game"
                    LANGUAGES CXX)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR})
list(APPEND CMAKE_PREFIX_PATH ${CMAKE_BINARY_DIR})                    

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
  message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
  file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/master/conan.cmake"
                "${CMAKE_BINARY_DIR}/conan.cmake"
                TLS_VERIFY ON)
endif()

include(FindSDL)
include(${CMAKE_BINARY_DIR}/conan.cmake)

conan_add_remote(NAME bincrafters
                 INDEX 1
                 URL https://api.bintray.com/conan/bincrafters/public-conan
                 VERIFY_SSL True)

conan_cmake_run(CONANFILE conanfile.txt
    BASIC_SETUP CMAKE_TARGETS
    BUILD missing
    ENV CONAN_CMAKE_GENERATOR="${CMAKE_GENERATOR}"
    ENV CONAN_MAKE_PROGRAM="${CMAKE_MAKE_PROGRAM}"
    ENV CC="${CMAKE_C_COMPILER}"
    ENV CXX="${CMAKE_CXX_COMPILER}"
)

find_package(LibXml2 REQUIRED)
find_package(PhysFS REQUIRED)
find_package(SDL REQUIRED)
find_package(SDL_mixer REQUIRED)
find_package(SDL_image REQUIRED)
find_package(SDL_ttf REQUIRED)
find_package(ZLIB REQUIRED)
find_library(SDL_GFX_LIBRARIES
  NAMES SDL_gfx
  HINTS
    ENV SDL_LIBRARIES
)

configure_file(config.h.in config.h @ONLY)

add_executable(lincity-ng
  src/gui/Rect2D.cpp
  src/gui/Paragraph.hpp
  src/gui/TableLayout.hpp
  src/gui/DocumentImage.hpp
  src/gui/FilledRectangle.hpp
  src/gui/TooltipManager.hpp
  src/gui/SwitchComponent.cpp
  src/gui/Event.hpp
  src/gui/Document.hpp
  src/gui/Gradient.hpp
  src/gui/ComponentFactory.hpp
  src/gui/FontManager.hpp
  src/gui/DocumentElement.hpp
  src/gui/Button.cpp
  src/gui/Image.cpp
  src/gui/Child.cpp
  src/gui/ScrollView.hpp
  src/gui/ScrollBar.cpp
  src/gui/Style.cpp
  src/gui/Desktop.hpp
  src/gui/Window.cpp
  src/gui/Component.cpp
  src/gui/ComponentLoader.hpp
  src/gui/PainterGL/TextureGL.hpp
  src/gui/PainterGL/PainterGL.hpp
  src/gui/PainterGL/TextureManagerGL.cpp
  src/gui/PainterGL/TextureManagerGL.hpp
  src/gui/PainterGL/TextureGL.cpp
  src/gui/PainterGL/PainterGL.cpp
  src/gui/XmlReader.hpp
  src/gui/TextureManager.hpp
  src/gui/Filter.cpp
  src/gui/Color.cpp
  src/gui/Panel.cpp
  src/gui/Window.hpp
  src/gui/PainterSDL/PainterSDL.cpp
  src/gui/PainterSDL/TextureManagerSDL.hpp
  src/gui/PainterSDL/TextureSDL.cpp
  src/gui/PainterSDL/TextureManagerSDL.cpp
  src/gui/PainterSDL/TextureSDL.hpp
  src/gui/PainterSDL/PainterSDL.hpp
  src/gui/ComponentLoader.cpp
  src/gui/Component.hpp
  src/gui/XmlReader.cpp
  src/gui/Texture.hpp
  src/gui/TextureManager.cpp
  src/gui/Painter.hpp
  src/gui/Filter.hpp
  src/gui/Color.hpp
  src/gui/Panel.hpp
  src/gui/Rect2D.hpp
  src/gui/DocumentImage.cpp
  src/gui/Paragraph.cpp
  src/gui/TableLayout.cpp
  src/gui/SwitchComponent.hpp
  src/gui/FilledRectangle.cpp
  src/gui/TooltipManager.cpp
  src/gui/callback/Signal.hpp
  src/gui/callback/CallbackBase.hpp
  src/gui/callback/Callback.hpp
  src/gui/Event.cpp
  src/gui/Gradient.cpp
  src/gui/Document.cpp
  src/gui/FontManager.cpp
  src/gui/ComponentFactory.cpp
  src/gui/Button.hpp
  src/gui/Child.hpp
  src/gui/Image.hpp
  src/gui/ScrollView.cpp
  src/gui/ScrollBar.hpp
  src/gui/Style.hpp
  src/gui/Desktop.cpp
  src/gui/Vector2.hpp

  src/lincity/lclib.cpp
  src/lincity/stats.cpp
  src/lincity/world.cpp
  src/lincity/engine.cpp
  src/lincity/engglobs.cpp
  src/lincity/fileutil.cpp
  src/lincity/lintypes.cpp
  src/lincity/loadsave.cpp
  src/lincity/simulate.cpp
  src/lincity/Vehicles.cpp
  src/lincity/init_game.cpp
  src/lincity/lc_locale.cpp
  src/lincity/transport.cpp
  src/lincity/xmlloadsave.cpp
  src/lincity/old_ldsvguts.cpp
  src/lincity/all_buildings.cpp
  src/lincity/UserOperation.cpp
  src/lincity/ConstructionCount.cpp
  src/lincity/ConstructionManager.cpp
  src/lincity/ConstructionRequest.cpp
  src/lincity/modules/tip.cpp
  src/lincity/modules/fire.cpp
  src/lincity/modules/mill.cpp
  src/lincity/modules/port.cpp
  src/lincity/modules/market.cpp
  src/lincity/modules/school.cpp
  src/lincity/modules/shanty.cpp
  src/lincity/modules/commune.cpp
  src/lincity/modules/cricket.cpp
  src/lincity/modules/oremine.cpp
  src/lincity/modules/pottery.cpp
  src/lincity/modules/recycle.cpp
  src/lincity/modules/coalmine.cpp
  src/lincity/modules/monument.cpp
  src/lincity/modules/parkland.cpp
  src/lincity/modules/water.cpp
  src/lincity/modules/windmill.cpp
  src/lincity/modules/residence.cpp
  src/lincity/modules/waterwell.cpp
  src/lincity/modules/windpower.cpp
  src/lincity/modules/blacksmith.cpp
  src/lincity/modules/coal_power.cpp
  src/lincity/modules/power_line.cpp
  src/lincity/modules/rocket_pad.cpp
  src/lincity/modules/substation.cpp
  src/lincity/modules/university.cpp
  src/lincity/modules/firestation.cpp
  src/lincity/modules/solar_power.cpp
  src/lincity/modules/organic_farm.cpp
  src/lincity/modules/health_centre.cpp
  src/lincity/modules/all_modules.cpp
  src/lincity/modules/heavy_industry.cpp
  src/lincity/modules/light_industry.cpp
  src/lincity/modules/track_road_rail.cpp

  src/lincity-ng/PbarInterface.cpp
  src/lincity-ng/TimerInterface.cpp
  src/lincity-ng/ButtonInterface.cpp
  src/lincity-ng/main.cpp
  src/lincity-ng/MapEdit.cpp
  src/lincity-ng/MainMenu.cpp
  src/lincity-ng/ScreenInterface.cpp
  src/lincity-ng/Dialog.cpp
  src/lincity-ng/Game.cpp
  src/lincity-ng/Config.cpp
  src/lincity-ng/MainLincity.cpp
  src/lincity-ng/Debug.cpp
  src/lincity-ng/readdir.cpp
  src/lincity-ng/getBundleSharePath.cpp
  src/lincity-ng/MiniMap.cpp
  src/lincity-ng/ButtonPanel.cpp
  src/lincity-ng/GameView.cpp
  src/lincity-ng/HelpWindow.cpp
  src/lincity-ng/MpsInterface.cpp
  src/lincity-ng/Util.cpp
  src/lincity-ng/Permutator.cpp
  src/lincity-ng/CheckButton.cpp
  src/lincity-ng/ReadPngInterface.cpp
  src/lincity-ng/Sound.cpp
  src/lincity-ng/Mps.cpp
  src/lincity-ng/ErrorInterface.cpp
  src/lincity-ng/PBar.cpp
  src/lincity-ng/EconomyGraph.cpp

  src/PhysfsStream/PhysfsSDL.cpp
  src/PhysfsStream/PhysfsSDL.hpp
  src/PhysfsStream/PhysfsStream.cpp
  src/PhysfsStream/PhysfsStream.hpp

  src/tinygettext/findlocale.cpp
  src/tinygettext/findlocale.cpp
  src/tinygettext/gettext.hpp
  src/tinygettext/tinygettext.cpp
  src/tinygettext/tinygettext.hpp
)
target_include_directories(lincity-ng PRIVATE
  ${CMAKE_INCLUDE_PATH}
  ${SDL_INCLUDE_DIRS}
  src
  src/gui
  src/gui_interface
  src/lincity
  ${CMAKE_CURRENT_BINARY_DIR}
)
target_link_libraries(lincity-ng PRIVATE
  ${LIBXML2_LIBRARIES}
  CONAN_PKG::physfs
  CONAN_PKG::spdlog
  ${SDL_LIBRARIES}
  ${SDL_GFX_LIBRARIES}
  ${SDL_IMAGE_LIBRARIES}
  ${SDL_MIXER_LIBRARIES}
  ${SDL_TTF_LIBRARIES}
  ${ZLIB_LIBRARIES}
)
if (APPLE)
  target_link_libraries(lincity-ng PRIVATE "-framework OpenGL")
endif()