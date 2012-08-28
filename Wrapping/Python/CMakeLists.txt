 # SWIG 1.3 is not enough because some unit tests failed with this version
FIND_PACKAGE(SWIG 2.0 REQUIRED)
FIND_PACKAGE(PythonLibs REQUIRED)
FIND_PACKAGE(NumPy REQUIRED)

INCLUDE(${SWIG_USE_FILE})

INCLUDE_DIRECTORIES(${PYTHON_INCLUDE_PATH})
INCLUDE_DIRECTORIES(${NUMPY_INCLUDE_DIR})
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})
INCLUDE_DIRECTORIES("${BTK_SOURCE_DIR}/Utilities/SWIG")

#SWIG_EXPORT_ITERATOR_METHODS
SET(CMAKE_SWIG_FLAGS "")
SET(CMAKE_SWIG_OUTDIR "${BTK_EXECUTABLE_PATH}")

SET(BTKSWIG_CPP_SRCS 
    btkCommonSwig.cpp
    btkIOSwig.cpp
    btkBasicFiltersSwig.cpp)
    
SET_SOURCE_FILES_PROPERTIES(btkSwig.i PROPERTIES CPLUSPLUS ON)
SET_SOURCE_FILES_PROPERTIES(btkSwig.i PROPERTIES SWIG_FLAGS "-includeall")

SWIG_ADD_MODULE(btk python btkSwig.i ${BTKSWIG_CPP_SRCS})

# Python extension modules on Windows must have the extension ".pyd" instead of ".dll" since Python 2.5.
IF(WIN32 AND NOT CYGWIN)
  SET_TARGET_PROPERTIES(${SWIG_MODULE_btk_REAL_NAME} PROPERTIES SUFFIX ".pyd")
ENDIF(WIN32 AND NOT CYGWIN)

SET(BTK_SWIG_LINK_LIBRARIES
    BTKCommon
    BTKIO
    BTKBasicFilters)

SWIG_LINK_LIBRARIES(btk ${PYTHON_LIBRARIES} ${BTK_SWIG_LINK_LIBRARIES})