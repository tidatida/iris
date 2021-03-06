include_directories(
	${CMAKE_CURRENT_BINARY_DIR}
	corelib
	noncore
	noncore/cutestuff
	noncore/legacy
	${QCA_INCLUDES}
	)

set( CMAKE_MODULE_PATH
	"${CMAKE_MODULE_PATH}"
	"${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules"
	"${CMAKE_CURRENT_SOURCE_DIR}/../cmake/modules"
	)

set(PLAIN_SOURCES
	corelib/irisnetglobal.cpp
	corelib/irisnetplugin.cpp

	noncore/icetransport.cpp
	noncore/stunmessage.cpp
	noncore/stuntypes.cpp
	noncore/stunutil.cpp

	noncore/cutestuff/bytestream.cpp
	noncore/cutestuff/httpconnect.cpp
	noncore/cutestuff/httppoll.cpp
	noncore/cutestuff/socks.cpp

	noncore/legacy/ndns.cpp
	noncore/legacy/safedelete.cpp
	noncore/legacy/servsock.cpp
	noncore/legacy/srvresolver.cpp
	)

set(SOURCES
	corelib/addressresolver.cpp
	corelib/netavailability.cpp
	corelib/netinterface.cpp
	corelib/netnames.cpp
	corelib/netnames_jdns.cpp
	corelib/objectsession.cpp

	noncore/ice176.cpp
	noncore/icecomponent.cpp
	noncore/icelocaltransport.cpp
	noncore/iceturntransport.cpp
	noncore/processquit.cpp
	noncore/stunallocate.cpp
	noncore/stunbinding.cpp
	noncore/stuntransaction.cpp
	noncore/turnclient.cpp
	noncore/udpportreserver.cpp

	noncore/cutestuff/bsocket.cpp
	)

if(WIN32)
	list(APPEND SOURCES corelib/netinterface_win.cpp)
elseif(UNIX)
	list(APPEND SOURCES corelib/netinterface_unix.cpp)
endif()

set(PLAIN_HEADERS
	corelib/irisnetexport.h
	corelib/irisnetglobal.h
	corelib/irisnetglobal_p.h

	noncore/stunmessage.h
	noncore/stuntypes.h
	noncore/stunutil.h
	)

set(HEADERS
	corelib/addressresolver.h
	corelib/irisnetplugin.h
	corelib/netavailability.h
	corelib/netinterface.h
	corelib/netnames.h
	corelib/objectsession.h

	noncore/ice176.h
	noncore/icecomponent.h
	noncore/icelocaltransport.h
	noncore/icetransport.h
	noncore/iceturntransport.h
	noncore/processquit.h
	noncore/stunallocate.h
	noncore/stunbinding.h
	noncore/stuntransaction.h
	noncore/turnclient.h
	noncore/udpportreserver.h

	noncore/cutestuff/bsocket.h
	noncore/cutestuff/bytestream.h
	noncore/cutestuff/httpconnect.h
	noncore/cutestuff/httppoll.h
	noncore/cutestuff/socks.h

	noncore/legacy/ndns.h
	noncore/legacy/safedelete.h
	noncore/legacy/servsock.h
	noncore/legacy/srvresolver.h
	)

qt_wrap_cpp(MOC_SOURCES ${HEADERS} ${SOURCES})
add_library(irisnet STATIC ${SOURCES} ${HEADERS} ${MOC_SOURCES} ${PLAIN_SOURCES} ${PLAIN_HEADERS} )

target_link_libraries(irisnet ${QJDns_LIBRARY})
if(USE_QT5)
	target_link_libraries(irisnet Qt5::Core Qt5::Network Qt5::Xml ${qca_LIB})
else()
	target_link_libraries(irisnet ${QT_LIBRARIES} ${qca_LIB})
endif()
