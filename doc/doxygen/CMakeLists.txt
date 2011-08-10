## Process this file with cmake
#====================================================================
#  NeXus - Neutron & X-ray Common Data Format
#  
#  CMakeLists for building the NeXus library and applications.
#
#  Copyright (C) 2011 Stephen Rankin
#  
#  This library is free software; you can redistribute it and/or
#  modify it under the terms of the GNU Lesser General Public
#  License as published by the Free Software Foundation; either
#  version 2 of the License, or (at your option) any later version.
# 
#  This library is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
#  Lesser General Public License for more details.
# 
#  You should have received a copy of the GNU Lesser General Public
#  License along with this library; if not, write to the Free 
#  Software Foundation, Inc., 59 Temple Place, Suite 330, Boston, 
#  MA  02111-1307  USA
#             
#  For further information, see <http://www.neutron.anl.gov/NeXus/>
#
#
#====================================================================

if (DOXYGEN_FOUND AND DOXYGEN_DOT_FOUND)

    SET (ENV{TOPSRC} ${CMAKE_SOURCE_DIR})

    ADD_CUSTOM_COMMAND( 
        OUTPUT    html-c latex-c c-api.tag
        COMMAND   ${DOXYGEN_EXECUTABLE}
        ARGS      ${CMAKE_SOURCE_DIR}/doc/doxygen/Doxyfile_c
        DEPENDS   ${javaclass}
        COMMENT   "C DOXYGEN"
    )

    ADD_CUSTOM_COMMAND( 
        OUTPUT    html-cpp latex-cpp
        COMMAND   ${DOXYGEN_EXECUTABLE}
        ARGS      ${CMAKE_SOURCE_DIR}/doc/doxygen/Doxyfile_cpp
        DEPENDS   html-c latex-c c-api.tag
        COMMENT   "CPP DOXYGEN"
    )

    ADD_CUSTOM_COMMAND( 
        OUTPUT    html-java latex-java
        COMMAND   ${DOXYGEN_EXECUTABLE}
        ARGS      ${CMAKE_SOURCE_DIR}/doc/doxygen/Doxyfile_java
        DEPENDS   html-cpp latex-cpp
        COMMENT   "Java DOXYGEN"
    )

    ADD_CUSTOM_COMMAND( 
        OUTPUT    html-python latex-python
        COMMAND   ${DOXYGEN_EXECUTABLE}
        ARGS      ${CMAKE_SOURCE_DIR}/doc/doxygen/Doxyfile_python
        DEPENDS   html-java latex-java
        COMMENT   "Python DOXYGEN"
    )

    ADD_CUSTOM_COMMAND( 
        OUTPUT    html-f90 latex-f90
        COMMAND   ${DOXYGEN_EXECUTABLE}
        ARGS      ${CMAKE_SOURCE_DIR}/doc/doxygen/Doxyfile_f90
        DEPENDS   html-java latex-java
        COMMENT   "F90 DOXYGEN"
    )

    ADD_CUSTOM_COMMAND( 
        OUTPUT    html-f77 latex-f77
        COMMAND   ${DOXYGEN_EXECUTABLE}
        ARGS      ${CMAKE_SOURCE_DIR}/doc/doxygen/Doxyfile_f77
        DEPENDS   html-f90 latex-f90
        COMMENT   "F77 DOXYGEN"
    )

    ADD_CUSTOM_TARGET(NexusDoxygenBuild ALL echo
        DEPENDS   html-f77 latex-f77
    )

    install (DIRECTORY html-c html-cpp html-java html-python html-f90 html-f77 DESTINATION ${NXDOCDIR}/doxygen)

endif (DOXYGEN_FOUND AND DOXYGEN_DOT_FOUND)
