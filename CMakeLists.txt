################################################################################
# <summary>        CMakeLists.txt for MPI.                            </summary>
# <date>           2013/08/03                                         </date>
# <author>         Quan Tran Minh.                                    </author>
# <email>          quantm@unist.ac.kr                                 </email>
################################################################################
CMAKE_MINIMUM_REQUIRED(VERSION 2.6 FATAL_ERROR)
PROJECT(samples_mpi)

FIND_PACKAGE(MPI REQUIRED)
FIND_PACKAGE(CUDA REQUIRED)
SET (EXECUTABLE_OUTPUT_PATH "${CMAKE_SOURCE_DIR}/bin/")
################################################################################
ENABLE_TESTING()
################################################################################
# MPI is just like regular C/C++
ADD_SUBDIRECTORY(hello_world)     
ADD_SUBDIRECTORY(hello_world_mpi)                  
ADD_SUBDIRECTORY(bandwidth)                  
################################################################################
#Clean the output and test logs
ADD_CUSTOM_TARGET(clear 
	COMMAND rm			
	`find -iname '*Make*' -not -name CMakeLists.txt` 
	`find -iname 'bin'`
	`find -iname 'Testing'`
	-rf)                                        
################################################################################
