CMAKE_MINIMUM_REQUIRED(VERSION 3.7)
PROJECT(main)
FIND_PACKAGE(LATEX)
SET(PDFLATEX_COMPILER xelatex)
SET(LATEX_DEFAULT_BUILD pdf)
INCLUDE(UseLATEX.cmake)
SET(parts
    espd.cls # Класс настроек ЕСПД
    preamble.tex 
    algo.tex
    abbreviations.tex
    src/аббревиатуры.tex    
    src/referat.tex
    src/введение.tex
    src/объект_исследования.tex
    src/средство_исследования.tex
    src/результаты.tex
    src/заключение.tex
    src/приложение1.tex
)
ADD_LATEX_DOCUMENT(
    main.tex
    BIBFILES bibliography.bib
    IMAGE_DIRS images
    INPUTS ${parts}    
)

add_custom_command(
        TARGET main
        COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_SOURCE_DIR}/build/main.pdf
                ${CMAKE_SOURCE_DIR}/output/лабораторный_отчет.pdf)
                
# Архивирование документации
add_custom_target(
custom_target
COMMAND ${CMAKE_COMMAND} -E tar cfvz "лабораторный_отчет.tgz"
"${CMAKE_CURRENT_SOURCE_DIR}/output/*.pdf"
WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/output"
)
install(CODE "execute_process(COMMAND make custom_target)")
install(SCRIPT "${CMAKE_CURRENT_SOURCE_DIR}/script.cmake")
                
