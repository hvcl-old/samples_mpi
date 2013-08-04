################################################################################
# <summary>        CMakeLists.txt for MPI.                            </summary>
# <date>           2013/08/03                                         </date>
# <author>         Quan Tran Minh.                                    </author>
# <email>          quantm@unist.ac.kr                                 </email>
################################################################################
if(CUDA_FOUND)
	MESSAGE("CUDA is found on this system.")
	list(APPEND CUDA_NVCC_FLAGS 
		-Xcompiler;
		-Wall; 
		-gencode;arch=compute_20,code=sm_20;
		--disable-warnings)
else(CUDA_FOUND)
    MESSAGE("CUDA is not installed on this system.")
endif()
if(MPI_FOUND)
	MESSAGE("MPI is found on this system.")
	INCLUDE_DIRECTORIES(${MPI_INCLUDE_PATH})
	list(APPEND CUDA_NVCC_FLAGS 
		-Xcompiler; 
		-lmpi)	
else(MPI_FOUND)
    MESSAGE("MPI is not installed on this system.")
endif()
################################################################################
CUDA_ADD_EXECUTABLE(bandwidth bandwidth.cu)
TARGET_LINK_LIBRARIES(bandwidth ${MPI_LIBRARIES})
ADD_TEST(Test_Bandwidth
	${MPIEXEC} 
	${MPIEXEC_NUMPROC_FLAG} 8
	${MPIEXEC_PREFLAGS} --host node001 
	${EXECUTABLE_OUTPUT_PATH}/bandwidth : 
	${MPIEXEC_NUMPROC_FLAG} 8
	${MPIEXEC_PREFLAGS} --host node002 
	${EXECUTABLE_OUTPUT_PATH}/bandwidth : 
	${MPIEXEC_NUMPROC_FLAG} 1
	${MPIEXEC_PREFLAGS} --host head
	${EXECUTABLE_OUTPUT_PATH}/bandwidth
	${MPIEXEC_POSTFLAGS} | sort)
################################################################################