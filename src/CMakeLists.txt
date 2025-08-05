cmake_minimum_required(VERSION 3.16)
project(vulkan_guide)

# Add source files
add_executable(vulkan_guide
    main.cpp
    vk_engine.cpp
    vk_engine.h
    vk_types.h
    vk_initializers.cpp
    vk_initializers.h)

# SDL2 handmatig toevoegen
set(SDL2_DIR "C:/lib/SDL2")  # Pas dit pad aan als nodig
include_directories(${SDL2_DIR}/include)
link_directories(${SDL2_DIR}/lib/x64)  # Of x86 als je 32-bit bouwt

# SDL2 bibliotheken linken
target_link_libraries(vulkan_guide SDL2 SDL2main)

# Vulkan, externe libs
target_link_libraries(vulkan_guide Vulkan::Vulkan)
target_link_libraries(vulkan_guide vkbootstrap vma glm tinyobjloader imgui stb_image)

# Zorg dat werkmap juist is in Visual Studio
set_property(TARGET vulkan_guide PROPERTY VS_DEBUGGER_WORKING_DIRECTORY "$<TARGET_FILE_DIR:vulkan_guide>")

# Voeg eventueel huidige source dir toe aan includes
target_include_directories(vulkan_guide PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}")

# Shaders afhankelijkheid
add_dependencies(vulkan_guide Shaders)

add_custom_command(TARGET vulkan_guide POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${SDL2_DIR}/lib/x64/SDL2.dll"
        $<TARGET_FILE_DIR:vulkan_guide>)