add_library(f5-json-schema
        annotations.cpp
        assertions.cpp
        assertions.array.cpp
        assertions.numeric.cpp
        assertions.object.cpp
        assertions.string.cpp
        schema.cpp
        schema.cache.cpp
        schema.loaders.cpp
        validator.cpp
    )
target_include_directories(f5-json-schema PUBLIC ../include)
target_link_libraries(f5-json-schema fost-inet)
if(CMAKE_VERSION VERSION_LESS "3.8")
    message(WARNING "This verison of cmake is too old to request C++17 for f5-json-schema.")
    target_compile_options(f5-json-schema INTERFACE -std=c++17)
else()
    target_compile_features(f5-json-schema INTERFACE cxx_std_17)
endif()
install(TARGETS f5-json-schema
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)
install(DIRECTORY ../include/f5 DESTINATION include)
