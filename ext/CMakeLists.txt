SET (libsolvext_SRCS
    solv_xfopen.c testcase.c)

SET (libsolvext_HEADERS
    tools_util.h solv_xfopen.h testcase.h)

IF (ENABLE_RPMDB)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
        pool_fileconflicts.c repo_rpmdb.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
        pool_fileconflicts.h repo_rpmdb.h)
ENDIF (ENABLE_RPMDB)

IF (ENABLE_RPMDB_PUBKEY)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_rpmdb_pubkey.c)
    SET (libsolvext_HEADERS ${libsolvext_SRCS}
	repo_rpmdb_pubkey.h)
ENDIF (ENABLE_RPMDB_PUBKEY)

IF (ENABLE_RPMMD)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_repomdxml.c repo_rpmmd.c
	repo_deltainfoxml.c repo_updateinfoxml.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_repomdxml.h repo_rpmmd.h
	repo_deltainfoxml.h repo_updateinfoxml.h)
ENDIF (ENABLE_RPMMD)

IF (ENABLE_SUSEREPO)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_content.c repo_products.c repo_releasefile_products.c
	repo_susetags.c repo_zyppdb.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_content.h repo_products.h repo_releasefile_products.h
	repo_susetags.h repo_zyppdb.h)
ENDIF (ENABLE_SUSEREPO)

IF (ENABLE_COMPS)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_comps.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_comps.h)
ENDIF (ENABLE_COMPS)

IF (ENABLE_DEBIAN)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_deb.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_deb.h)
ENDIF (ENABLE_DEBIAN)
	
IF (ENABLE_HELIXREPO)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_helix.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_helix.h)
ENDIF (ENABLE_HELIXREPO)

IF (ENABLE_MDKREPO)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_mdk.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_mdk.h)
ENDIF (ENABLE_MDKREPO)

IF (ENABLE_ARCHREPO)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_arch.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_arch.h)
ENDIF (ENABLE_ARCHREPO)

IF (ENABLE_CUDFREPO)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_cudf.c)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_cudf.h)
ENDIF (ENABLE_CUDFREPO)

IF (ENABLE_HAIKU)
    SET (libsolvext_SRCS ${libsolvext_SRCS}
	repo_haiku.cpp)
    SET (libsolvext_HEADERS ${libsolvext_HEADERS}
	repo_haiku.h)
ENDIF (ENABLE_HAIKU)

SET (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
SET (CMAKE_SHARED_LINKER_FLAGS "${LINK_FLAGS} -Wl,--version-script=${CMAKE_SOURCE_DIR}/ext/libsolvext.ver")

IF (DISABLE_SHARED)
ADD_LIBRARY (libsolvext STATIC ${libsolvext_SRCS})
ELSE (DISABLE_SHARED)
ADD_LIBRARY (libsolvext SHARED ${libsolvext_SRCS})
TARGET_LINK_LIBRARIES(libsolvext libsolv ${SYSTEM_LIBRARIES})
ENDIF (DISABLE_SHARED)

SET_TARGET_PROPERTIES(libsolvext PROPERTIES OUTPUT_NAME "solvext")
SET_TARGET_PROPERTIES(libsolvext PROPERTIES SOVERSION ${LIBSOLVEXT_SOVERSION})

INSTALL (FILES ${libsolvext_HEADERS} DESTINATION "${CMAKE_INSTALL_PREFIX}/include/solv")
INSTALL (TARGETS libsolvext LIBRARY DESTINATION ${LIB_INSTALL_DIR} ARCHIVE DESTINATION ${LIB_INSTALL_DIR})

IF (ENABLE_STATIC AND NOT DISABLE_SHARED)
ADD_LIBRARY (libsolvext_static STATIC ${libsolvext_SRCS})
SET_TARGET_PROPERTIES(libsolvext_static PROPERTIES OUTPUT_NAME "solvext")
SET_TARGET_PROPERTIES(libsolvext_static PROPERTIES SOVERSION ${LIBSOLVEXT_SOVERSION})
INSTALL (TARGETS libsolvext_static LIBRARY DESTINATION ${LIB_INSTALL_DIR} ARCHIVE DESTINATION ${LIB_INSTALL_DIR})
ENDIF (ENABLE_STATIC AND NOT DISABLE_SHARED)
