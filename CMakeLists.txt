cmake_minimum_required(VERSION 3.31)
project(SDL2_Template)

set(CMAKE_CXX_STANDARD 20)

include(FetchContent)

FetchContent_Declare(
        SDL2
        GIT_REPOSITORY https://github.com/libsdl-org/SDL.git
        GIT_TAG        release-2.32.4
)

# Optional: Disable things not needed to speed up the build
set(SDL_TEST OFF CACHE BOOL "" FORCE)
set(SDL_STATIC ON CACHE BOOL "" FORCE)
set(SDL_SHARED OFF CACHE BOOL "" FORCE)

FetchContent_MakeAvailable(SDL2)

add_executable(SDL2_Template src/main.cpp)


target_link_libraries(SDL2_Template PRIVATE SDL2::SDL2-static)
target_include_directories(SDL2_Template PRIVATE "${sdl2_SOURCE_DIR}/include")

# On macOS, SDL2 requires these system frameworks
if(APPLE)
    target_link_libraries(SDL2_Template PRIVATE
            "-framework Cocoa"
            "-framework CoreVideo"
            "-framework IOKit"
            "-framework ForceFeedback"
            "-framework Carbon"
            "-framework CoreAudio"
            "-framework AudioToolbox"
    )
endif()