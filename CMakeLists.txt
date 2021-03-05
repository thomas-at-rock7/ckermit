cmake_minimum_required(VERSION "3.13.4")
project("kermit")

if (CMAKE_BUILD_TYPE)
    if (${CMAKE_BUILD_TYPE} STREQUAL "Release")
        message(STATUS "Build type Release")
        add_definitions(-DNDEBUG) # Diable Assert functions
    else()
        message(STATUS "Build type Debug")
        add_definitions(-DDEBUG)
    endif()
else()
    message(STATUS "Build type not defined, defaulting to a Release build")
    add_definitions(-DNDEBUG) # Diable Assert functions
endif()

set (CKCPRO_TARGET "${CMAKE_CURRENT_SOURCE_DIR}/ckcpro.c")
set (WART_BINARY "wart")

add_executable(${WART_BINARY} ckwart.c)
add_custom_command(OUTPUT ${CKCPRO_TARGET}
                   COMMAND ${WART_BINARY} "${CMAKE_CURRENT_SOURCE_DIR}/ckcpro.w" ${CKCPRO_TARGET}
                   WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                   DEPENDS ${WART_BINARY})

set (CMAKE_C_FLAGG "${CMAKE_C_FLAGS} -O -pipe -funsigned-char")
add_definitions(-DLINUX -DFNFLOAT -DCK_POSIX_SIG -DCK_NEWTERM -DTCPSOCKET -DLINUXFSSTND -DNOCOTFMC -DPOSIX -DUSE_STRERROR -DCK_NCURSES -DHAVE_PTMX  -DHAVE_CRYPT_H  -DHAVE_OPENPTY -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64  -DKTARGET="linux")

find_library(LIB_UTIL util)
find_library(LIB_NCURSES ncurses)
find_library(LIB_RESOLV resolv)
find_library(LIB_CRYPT crypt)
find_library(LIB_M m)

add_executable(${CMAKE_PROJECT_NAME}
               ckcpro.c
               ckcmai.c
               ckclib.c
               ckutio.c
               ckufio.c
               ckcfns.c
               ckcfn2.c
               ckcfn3.c
               ckuxla.c
               ckucmd.c
               ckuus2.c
               ckuus3.c
               ckuus4.c
               ckuus5.c
               ckuus6.c
               ckuus7.c
               ckuusx.c
               ckuusy.c
               ckuusr.c
               ckucns.c
               ckudia.c
               ckuscr.c
               ckcnet.c
               ckusig.c
               ckctel.c
               ckcuni.c
               ckupty.c
               ckcftp.c
               ckuath.c
               ck_crp.c
               ck_ssl.c)

target_link_libraries(${CMAKE_PROJECT_NAME} PUBLIC ${LIB_UTIL} ${LIB_NCURSES} ${LIB_RESOLV} ${LIB_CRYPT} ${LIB_M})
install(TARGETS ${CMAKE_PROJECT_NAME} DESTINATION bin)