if(TARGET stress)
    set_property(TARGET stress PROPERTY EXCLUDE_FROM_ALL TRUE)

    add_executable(json-schema-testsuite-v7-runner EXCLUDE_FROM_ALL testsuite.cpp)
    target_link_libraries(json-schema-testsuite-v7-runner f5-json-schema fost-cli fost-inet)
    add_dependencies(stress json-schema-testsuite-v7-runner)
    set(json-schema-testsuite-v7-parts)

    ## The use of a macro rather than a function gets around all sorts of
    ## weird cmake stuff to do with the scoping rules of appending to the
    ## `json-schema-testsuite-v7-parts` list.
    macro(draftv7 name)
        add_custom_command(OUTPUT json-schema-testsuite-v7-${name}
                COMMAND json-schema-testsuite-v7-runner -b false
                    "-o" json-schema-testsuite-v7-${name}
                    "-p" ${CMAKE_CURRENT_SOURCE_DIR}/../checks/json-schema.schema.json
                    ${name}.json
                DEPENDS json-schema-testsuite-v7-runner)
        list(APPEND json-schema-testsuite-v7-parts json-schema-testsuite-v7-${name})
    endmacro()

    draftv7(additionalItems)
    draftv7(additionalProperties)
    draftv7(allOf)
    draftv7(anyOf)
    draftv7(boolean_schema)
    draftv7(const)
    draftv7(contains)
    draftv7(default)
    draftv7(definitions)
    draftv7(dependencies)
    draftv7(enum)
    draftv7(exclusiveMaximum)
    draftv7(exclusiveMinimum)
    draftv7(if-then-else)
    draftv7(items)
    draftv7(maximum)
    draftv7(maxItems)
    draftv7(maxLength)
    draftv7(maxProperties)
    draftv7(minimum)
    draftv7(minItems)
    draftv7(minLength)
    draftv7(minProperties)
    draftv7(multipleOf)
    draftv7(not)
    draftv7(oneOf)
    draftv7(pattern)
    draftv7(patternProperties)
    draftv7(properties)
    draftv7(propertyNames)
    draftv7(ref)
    draftv7(refRemote)
    draftv7(required)
    draftv7(type)
    draftv7(uniqueItems)


    add_custom_target(json-schema-testsuite-v7
        DEPENDS ${json-schema-testsuite-v7-parts})
    set_property(TARGET json-schema-testsuite-v7 PROPERTY EXCLUDE_FROM_ALL TRUE)
    add_dependencies(stress json-schema-testsuite-v7)
endif()
