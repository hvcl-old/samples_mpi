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
ADD_EXECUTABLE(hello_world main.cpp)
TARGET_LINK_LIBRARIES(hello_world ${MPI_LIBRARIES})
ADD_TEST(Test_Hello_World_Regular  
	${EXECUTABLE_OUTPUT_PATH}/hello_world)	
ADD_TEST(Test_Hello_World_Multiple 
	${MPIEXEC} 
	${MPIEXEC_NUMPROC_FLAG} 1
	${EXECUTABLE_OUTPUT_PATH}/hello_world)
################################################################################