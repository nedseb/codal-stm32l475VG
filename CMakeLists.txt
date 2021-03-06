project(codal-stm32l475)

include("${CODAL_UTILS_LOCATION}")
# find sources and headers
RECURSIVE_FIND_DIR(TOP_LEVEL_INCLUDE_DIRS0 "${CMAKE_CURRENT_SOURCE_DIR}/stm32cube/stm32l4xx" "*.h")
RECURSIVE_FIND_DIR(TOP_LEVEL_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/inc" "*.h")
RECURSIVE_FIND_DIR(INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/${CODAL_OUTPUT_NAME}/inc" "*.h")

RECURSIVE_FIND_FILE(TOP_LEVEL_SOURCE1 "${CMAKE_CURRENT_SOURCE_DIR}/source" "*.c")
RECURSIVE_FIND_FILE(TOP_LEVEL_SOURCE0 "${CMAKE_CURRENT_SOURCE_DIR}/stm32cube/stm32l4xx" "*.[cs]")
RECURSIVE_FIND_FILE(TOP_LEVEL_SOURCE "${CMAKE_CURRENT_SOURCE_DIR}/source" "*.cpp")
LIST(APPEND TOP_LEVEL_SOURCE "${CMAKE_CURRENT_SOURCE_DIR}/asm/CortexContextSwitch.s")

RECURSIVE_FIND_FILE(OBJECT_FILES "${CMAKE_CURRENT_SOURCE_DIR}/${CODAL_OUTPUT_NAME}/lib" "*.o")
RECURSIVE_FIND_FILE(ARCHIVE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/${CODAL_OUTPUT_NAME}/lib" "*.a")

# add them
include_directories(${TOP_LEVEL_INCLUDE_DIRS0} ${TOP_LEVEL_INCLUDE_DIRS} ${INCLUDE_DIRS})

# create our target
add_library(codal-stm32l475 ${TOP_LEVEL_SOURCE1} ${TOP_LEVEL_SOURCE0} ${TOP_LEVEL_SOURCE} )

target_link_libraries(codal-stm32l475 codal-core ${OBJECT_FILES} ${ARCHIVE_FILES})

# expose it to parent cmake.
target_include_directories(codal-stm32l475 PUBLIC ${TOP_LEVEL_INCLUDE_DIRS0} ${TOP_LEVEL_INCLUDE_DIRS} ${INCLUDE_DIRS})
