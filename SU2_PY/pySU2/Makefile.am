################################################################################
#
# \file Makefile.am
# \brief Makefile for the SU2 Python-wrapper
# \author D. Thomas
# \version 4.3.0 "Cardinal"
#
# SU2 Lead Developers: Dr. Francisco Palacios (Francisco.D.Palacios@boeing.com).
#                      Dr. Thomas D. Economon (economon@stanford.edu).
#
# SU2 Developers: Prof. Juan J. Alonso's group at Stanford University.
#                 Prof. Piero Colonna's group at Delft University of Technology.
#                 Prof. Nicolas R. Gauger's group at Kaiserslautern University of Technology.
#                 Prof. Alberto Guardone's group at Polytechnic University of Milan.
#                 Prof. Rafael Palacios' group at Imperial College London.
#                 Prof. Edwin van der Weide's group at the University of Twente.
#                 Prof. Vincent Terrapon's group at the University of Liege.
#
# Copyright (C) 2012-2016 SU2, the open-source CFD code.
#
# SU2 is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation; either
# version 2.1 of the License, or (at your option) any later version.
#
# SU2 is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
# Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with SU2. If not, see <http://www.gnu.org/licenses/>.
#
################################################################################


CURRENT_DIR = ${abs_top_builddir}/SU2_PY/pySU2

SO_EXT=so

SWIG_SRC = ${abs_top_srcdir}/SU2_PY/pySU2/pySU2.i
SWIG_WRAP_REAL = SU2_APIPYTHON_wrap
SWIG_SO_REAL = _SU2Solver.${SO_EXT}

PYTHON_INCLUDE = -I/usr/include/python2.7
NUMPY_INCLUDE = /usr/lib/python2.7/dist-packages/numpy/core/include
#MPI4PY_INCLUDE = /usr/lib/python2.7/dist-packages/mpi4py/include

SWIG = swig

pySU2_INCLUDE = -I${abs_top_srcdir}/Common/include \
	-I${abs_top_srcdir}/SU2_CFD/include \
	-I${abs_top_srcdir}/SU2_DEF/include \
	-I${abs_top_srcdir}/SU2_DOT/include \
	-I${abs_top_srcdir}/SU2_DOT/include \
	-I${abs_top_srcdir}/SU2_GEO/include \
	-I${abs_top_srcdir}/SU2_MSH/include \
	-I${abs_top_srcdir}/SU2_SOL/include


#PY_INCLUDE = ${PYTHON_INCLUDE} -I${NUMPY_INCLUDE} -I${MPI4PY_INCLUDE}
PY_INCLUDE = ${PYTHON_INCLUDE} -I${NUMPY_INCLUDE}
SWIG_INCLUDE = -I${abs_top_srcdir}/SU2_CFD/include \
		-I${abs_top_srcdir}/Common/include

SUBDIR_EXEC = ${bindir}

all:	
	@echo "******** Wrapping SU2 ********"; \
	${MAKE} real; 
	
# List the dependencies
real: ${SWIG_SO_REAL}

EXTRA_CC_FLAGS = -fPIC -O3

#External includes
METIS_INCLUDE = -I${abs_top_srcdir}/externals/metis/include
PARMETIS_INCLUDE = -I${abs_top_srcdir}/externals/parmetis/include
TECIO_INCLUDE = -I${abs_top_srcdir}/externals/tecio/include

#External libraries (METIS_LIB and PARMETIS_LIB are already defined)
TECIO_LIB = ${abs_top_builddir}/externals/tecio/libtecio.a

#SU2Core lib
SU2SOLVER_LIB = ${abs_top_builddir}/SU2_CFD/obj/libSU2Core.a \
		${abs_top_builddir}/Common/lib/libSU2.a

# Set the linking flags to use
pySU2_LD_FLAGS = ${SU2SOLVER_LIB} ${TECIO_LIB} ${PARMETIS_LIB} ${METIS_LIB} ${CGNS_LIB}

# Set the command line flags to use for compilation
pySU2_CC_FLAGS = ${EXTRA_CC_FLAGS} ${METIS_INCLUDE} ${PARMETIS_INCLUDE} ${TECIO_INCLUDE} ${CGNS_INCLUDE} ${pySU2_INCLUDE}

SO_LINK_FLAGS= -fPIC -shared 

# Default rule for creating the _wrap.cxx file from the .i file
${SWIG_WRAP_REAL}.cxx: ${SU2SOLVER_LIB} ${SWIG_SRC}
	${SWIG} ${TACS_DEF} -Wall ${SWIG_INCLUDE} -outdir ./ -o ${SWIG_WRAP_REAL}.cxx -c++ -python ${SWIG_SRC} 

#Default rule for compiling the .o files
${SWIG_WRAP_REAL}.o: ${SWIG_WRAP_REAL}.cxx
	${CXX} ${pySU2_CC_FLAGS} ${PY_INCLUDE} -c ${SWIG_WRAP_REAL}.cxx -o ${SWIG_WRAP_REAL}.o

#_%.${SO_EXT}: %.o
#	${CXX} ${SO_LINK_FLAGS} -o _$*.${SO_EXT} $*_wrap.o ${pySU2_LD_FLAGS}

${SWIG_SO_REAL}: ${SWIG_WRAP_REAL}.o
	${CXX} ${SO_LINK_FLAGS} -o ${SWIG_SO_REAL} ${SWIG_WRAP_REAL}.o ${pySU2_LD_FLAGS}

.SECONDARY:

clean: 
	rm -f ${CURRENT_DIR}/*.cxx
	rm -f ${CURRENT_DIR}/*.${SO_EXT}
	rm -f ${CURRENT_DIR}/*.o
	rm -f ${CURRENT_DIR}/*.h
	rm -f ${CURRENT_DIR}/*.py

install:
	/usr/bin/install -c  SU2Solver.py ${bindir}
	/usr/bin/install -c _SU2Solver.so ${bindir}

