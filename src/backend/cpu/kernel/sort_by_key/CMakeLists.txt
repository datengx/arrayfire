FILE(STRINGS "${CMAKE_CURRENT_SOURCE_DIR}/kernel/sort_by_key/sort_by_key_impl.cpp" FILESTRINGS)

FOREACH(STR ${FILESTRINGS})
    IF(${STR} MATCHES "// SBK_TYPES")
        STRING(REPLACE "// SBK_TYPES:" "" TEMP ${STR})
        STRING(REPLACE " " ";" SBK_TYPES ${TEMP})
    ENDIF()
ENDFOREACH()

FOREACH(SBK_TYPE ${SBK_TYPES})
    ADD_LIBRARY(cpu_sort_by_key_${SBK_TYPE} OBJECT
        "${CMAKE_CURRENT_SOURCE_DIR}/kernel/sort_by_key/sort_by_key_impl.cpp"
        "${CMAKE_CURRENT_SOURCE_DIR}/kernel/sort_by_key_impl.hpp")
    SET_TARGET_PROPERTIES(cpu_sort_by_key_${SBK_TYPE} PROPERTIES COMPILE_FLAGS "-DTYPE=${SBK_TYPE}")
    LIST(APPEND SORT_BY_KEY_OBJECTS $<TARGET_OBJECTS:cpu_sort_by_key_${SBK_TYPE}>)
ENDFOREACH(SBK_TYPE ${SBK_TYPES})
