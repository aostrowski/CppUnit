cmake_minimum_required(VERSION 2.8.12)
cmake_policy(SET CMP0048 NEW)

project(cppunit VERSION 1.14.2 LANGUAGES CXX)

if(NOT ${CMAKE_VERSION} VERSION_LESS 3.1)
    set(CMAKE_CXX_STANDARD 11)
else()
    set(CMAKE_CXX_FLAGS "-std=c++11")
endif()

set(Sources
    src/cppunit/AdditionalMessage.cpp
    src/cppunit/Asserter.cpp
    src/cppunit/BriefTestProgressListener.cpp
    src/cppunit/CompilerOutputter.cpp
    src/cppunit/DefaultProtector.cpp
    #src/cppunit/DllMain.cpp #Apparently this one isn't built.
    src/cppunit/DynamicLibraryManager.cpp
    src/cppunit/DynamicLibraryManagerException.cpp
    src/cppunit/Exception.cpp
    src/cppunit/Message.cpp
    src/cppunit/PlugInManager.cpp
    src/cppunit/PlugInParameters.cpp
    src/cppunit/Protector.cpp
    src/cppunit/ProtectorChain.cpp
    src/cppunit/RepeatedTest.cpp
    src/cppunit/ShlDynamicLibraryManager.cpp
    src/cppunit/SourceLine.cpp
    src/cppunit/StringTools.cpp
    src/cppunit/SynchronizedObject.cpp
    src/cppunit/Test.cpp
    src/cppunit/TestAssert.cpp
    src/cppunit/TestCase.cpp
    src/cppunit/TestCaseDecorator.cpp
    src/cppunit/TestComposite.cpp
    src/cppunit/TestDecorator.cpp
    src/cppunit/TestFactoryRegistry.cpp
    src/cppunit/TestFailure.cpp
    src/cppunit/TestLeaf.cpp
    src/cppunit/TestNamer.cpp
    src/cppunit/TestPath.cpp
    src/cppunit/TestPlugInDefaultImpl.cpp
    src/cppunit/TestResult.cpp
    src/cppunit/TestResultCollector.cpp
    src/cppunit/TestRunner.cpp
    src/cppunit/TestSetUp.cpp
    src/cppunit/TestSuccessListener.cpp
    src/cppunit/TestSuite.cpp
    src/cppunit/TestSuiteBuilderContext.cpp
    src/cppunit/TextOutputter.cpp
    src/cppunit/TextTestProgressListener.cpp
    src/cppunit/TextTestResult.cpp
    src/cppunit/TextTestRunner.cpp
    src/cppunit/TypeInfoHelper.cpp
    src/cppunit/UnixDynamicLibraryManager.cpp
    src/cppunit/Win32DynamicLibraryManager.cpp
    src/cppunit/XmlDocument.cpp
    src/cppunit/XmlElement.cpp
    src/cppunit/XmlOutputter.cpp
    src/cppunit/XmlOutputterHook.cpp
)

configure_file("config-auto.h.in" "cppunit/config-auto.h")

#Build cppunit's library.
add_library(cppunit STATIC ${Sources})
add_library(cppunit::cppunit ALIAS cppunit)
target_include_directories(cppunit
    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/cppunit>
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
        $<INSTALL_INTERFACE:include>)

#Installation.
include(GNUInstallDirs)
install(TARGETS cppunit
    EXPORT cppunit-targets
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(EXPORT cppunit-targets
    NAMESPACE cppunit::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/cppunit
)

install(
    DIRECTORY include
    DESTINATION .
    FILES_MATCHING PATTERN *.h
    REGEX "msvc6" EXCLUDE
)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/cppunit/config-auto.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/cppunit)

include(CMakePackageConfigHelpers)
configure_package_config_file(cppunitConfig.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/cppunitConfig.cmake INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/cppunit)
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/cppunitConfigVersion.cmake COMPATIBILITY SameMajorVersion)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/cppunitConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/cppunitConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/cppunit
)
