include_directories(${CMAKE_SOURCE_DIR}/src)

set(SOURCE_FILES
        cip/connectionManager/ForwardCloseRequest.cpp
        cip/connectionManager/ForwardOpenRequest.cpp
        cip/connectionManager/LargeForwardOpenRequest.cpp
        cip/connectionManager/ForwardOpenResponse.cpp
        cip/connectionManager/NetworkConnectionParametersBuilder.cpp

        cip/CipRevision.cpp
        cip/EPath.cpp
        cip/MessageRouterRequest.cpp
        cip/MessageRouterResponse.cpp
        cip/CipString.cpp

        eip/CommonPacket.cpp
        eip/CommonPacketItem.cpp
        eip/CommonPacketItemFactory.cpp
        eip/EncapsPacket.cpp
        eip/EncapsPacketFactory.cpp

        fileObject/FileObjectEmptyState.cpp
        fileObject/FileObjectLoadedState.cpp
        fileObject/FileObjectNonExistentState.cpp
        fileObject/FileObjectState.cpp
        fileObject/FileObjectUploadInProgressState.cpp

        sockets/BaseSocket.cpp
        sockets/EndPoint.cpp
        sockets/TCPSocket.cpp
        sockets/UDPBoundSocket.cpp
        sockets/UDPSocket.cpp

        utils/Logger.cpp
        utils/Buffer.cpp

        BaseObject.cpp
        ConnectionManager.cpp
        DiscoveryManager.cpp
        FileObject.cpp
        IdentityObject.cpp
        IOConnection.cpp
        MessageRouter.cpp
        ParameterObject.cpp
        SessionInfo.cpp)

# if vendor scripts are enabled
if(ENABLE_VENDOR_SRC)
	add_subdirectory(vendor)
endif()

add_library(EIPScanner SHARED ${SOURCE_FILES} ${VENDOR_FILES})
add_library(EIPScannerS STATIC ${SOURCE_FILES} ${VENDOR_FILES})

if(WIN32)
	target_link_libraries(EIPScanner ws2_32)
	target_link_libraries(EIPScannerS ws2_32)
endif()

set_target_properties(
        EIPScanner
        PROPERTIES
        VERSION ${EIPSCANNER_FULL_VERSION}
        SOVERSION ${EIPSCANNER_MAJOR_VERSION})

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)
# generate the version file for the config file
write_basic_package_version_file(
        ${CMAKE_CURRENT_BINARY_DIR}/EIPScannerConfigVersion.cmake
        VERSION ${EIPSCANNER_FULL_VERSION}
        COMPATIBILITY SameMajorVersion)

# generate cmake config file
configure_package_config_file(${CMAKE_SOURCE_DIR}/cmake/EIPScannerConfig.cmake.in
        EIPScannerConfig.cmake
        INSTALL_DESTINATION  ${CMAKE_INSTALL_LIBDIR}/cmake/EIPScanner
        INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX}/bin
        PATH_VARS CMAKE_INSTALL_INCLUDEDIR)

install(TARGETS EIPScanner EIPScannerS
        EXPORT EIPScannerTargets
        LIBRARY DESTINATION  ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION  ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_PREFIX}/include/EIPScanner)

install(DIRECTORY ${CMAKE_SOURCE_DIR}/src/
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/EIPScanner
        FILES_MATCHING PATTERN "*.h*")

install(EXPORT EIPScannerTargets
        FILE EIPScannerTargets.cmake
        NAMESPACE EIPScanner::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/EIPScanner)

install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/EIPScannerConfig.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/EIPScannerConfigVersion.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/EIPScanner)
