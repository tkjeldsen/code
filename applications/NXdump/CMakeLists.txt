## Process this file with cmake
#=============================================================================
#  NeXus - Neutron & X-ray Common Data Format
#
#  CMakeLists for building the NeXus library and applications.
#
#  Copyright (C) 2011 Stephen Rankin
#            (C) 2015 Eugen Wintersberger <eugen.wintersberger@desy.de>
#
#  This library is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by the
#  Free Software Foundation; either version 2 of the License, or (at your
#  option) any later version.
#
#  This library is distributed in the hope that it will be useful, but WITHOUT
#  ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
#  FITNESS FOR A PARTICULAR PURPOSE.  See the GNU Lesser General Public License
#  for more details.
#
#  You should have received a copy of the GNU Lesser General Public License
#  along with this library; if not, write to the Free Software Foundation,
#  Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
#
#  For further information, see <http://www.nexusformat.org>
#
#
#=============================================================================

#-----------------------------------------------------------------------------
# build and link the program binary
#-----------------------------------------------------------------------------
include_directories(${PROJECT_BINARY_DIR}/bindings/f90)
add_executable (nxdump NXdump.f90)
target_link_libraries(nxdump NeXus_Shared_Library NeXus_F90_Shared_Library)

#-----------------------------------------------------------------------------
# build the manpage
#-----------------------------------------------------------------------------
add_custom_command(OUTPUT nxdump.1
                   COMMAND rst2man ${CMAKE_CURRENT_SOURCE_DIR}/nxdump.1.rst 
                           > nxdump.1
                   DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/nxdump.1.rst
                   COMMENT "generate man page for nxdump")
                   
add_custom_target(nxdump_manpage ALL DEPENDS nxdump.1)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/nxdump.1
        DESTINATION ${CMAKE_INSTALL_MANDIR}/man1
        COMPONENT Documentation)

#-----------------------------------------------------------------------------
# install the program binary
#-----------------------------------------------------------------------------
install (TARGETS nxdump 
         DESTINATION ${CMAKE_INSTALL_BINDIR} 
         COMPONENT Runtime)



