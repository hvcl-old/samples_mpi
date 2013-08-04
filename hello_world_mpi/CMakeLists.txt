################################################################################
# <summary>        CMakeLists.txt for MPI.                            </summary>
# <date>           2013/08/03                                         </date>
# <author>         Quan Tran Minh.                                    </author>
# <email>          quantm@unist.ac.kr                                 </email>
################################################################################
if(MPI_FOUND)
	MESSAGE("MPI is found on this system.")
	INCLUDE_DIRECTORIES(${MPI_INCLUDE_PATH})
else(MPI_FOUND)
    MESSAGE("MPI is not installed on this system.")
endif()
################################################################################
ADD_EXECUTABLE(hello_world_mpi main.cpp)
TARGET_LINK_LIBRARIES(hello_world_mpi ${MPI_LIBRARIES})
ADD_TEST(Test_Hello_World_MPI_Call_By_Host
	${MPIEXEC} 
	${MPIEXEC_NUMPROC_FLAG} 10
	${MPIEXEC_PREFLAGS} --host head,node001,node002
	${EXECUTABLE_OUTPUT_PATH}/hello_world_mpi
	${MPIEXEC_POSTFLAGS} |sort)
ADD_TEST(Test_Hello_World_MPI_Call_By_Hostfile
	${MPIEXEC} 
	${MPIEXEC_NUMPROC_FLAG} 28
	${MPIEXEC_PREFLAGS} --machinefile ${CMAKE_CURRENT_BINARY_DIR}/machines.ib
	${EXECUTABLE_OUTPUT_PATH}/hello_world_mpi
	${MPIEXEC_POSTFLAGS} | sort)
ADD_TEST(Test_Hello_World_MPI_Call_By_Worker_Master
	${MPIEXEC} 
	${MPIEXEC_NUMPROC_FLAG} 8
	${MPIEXEC_PREFLAGS} --host node001 
	${EXECUTABLE_OUTPUT_PATH}/hello_world_mpi : 
	${MPIEXEC_NUMPROC_FLAG} 8
	${MPIEXEC_PREFLAGS} --host node002 
	${EXECUTABLE_OUTPUT_PATH}/hello_world_mpi : 
	${MPIEXEC_NUMPROC_FLAG} 1
	${MPIEXEC_PREFLAGS} --host head
	${EXECUTABLE_OUTPUT_PATH}/hello_world_mpi
	${MPIEXEC_POSTFLAGS} | sort)
################################################################################