###############################################################################
#
# Theron++
#
# This file builds the the various examples of the Theron++ framework
#
# Author: Geir Horn, University of Oslo, 2017
#
###############################################################################

#
# Defining compiler and commands
#

#CC = g++
CC = clang++
RM = rm -f

# Location of the Theron++ framework relative to this make file and the code

THERON = ..

# Since some of the utility classes still includes "Theron/Theron.h" is it 
# necessary to define this file as a symbolic link to the "Actor.hpp" file of
# Theron++ For the author's installation it is done at the following location

OLD_THERON = ../../Theron/Include

# Optimisation -O3 is the highest level of optimisation and should be used 
# with production code. -Og is the code optimising and offering debugging 
# transparency and should be use while the code is under development

OPTIMISATION_FLAG = 

# It is useful to let the compiler generate the dependencies for the various 
# files, and the following will produce .d files that can be included at the 
# end. The -MMD flag is equivalent with -MD, but the latter will include system 
# headers in the output (which we do not need here). The -MP includes an 
# empty rule to create the dependencies so that make would not create any errors 
# if the file name changes.

DEPENDENCY_FLAGS = -MMD -MP

# Options 

GENERAL_OPTIONS = -c -Wall -std=c++11 -ggdb -D_DEBUG
INCLUDE_DIRECTORIES = -I. -I/usr/include -I ${THERON} -I${OLD_THERON}

# Putting it together as the actual options given to the compiler and the 
# linker. Note that pthread is needed on Linux systems since it seems to 
# be the underlying implementation of std::thread. 

CFLAGS = $(DEPENDENCY_FLAGS) $(OPTIMISATION_FLAG) $(GENERAL_OPTIONS)
LDFLAGS = -Wl,--allow-multiple-definition -ggdb -D_DEBUG -pthread

# Then the Theron++ headers and source files are given. Note that the actor 
# file is given as the last source file so that it will be linked as the 
# last file since the other utility classes will be derived from the actor.
# Note also that the communication protocol is not included as it is not 
# generally needed.

THERON_HEADERS = Actor.hpp LinkMessage.hpp NetworkLayer.hpp \
	         SessionLayer.hpp PresentationLayer.hpp\
	         ConsolePrint.hpp EventHandler.hpp
THERON_SOURCE  = $(THERON)/ConsolePrint.cpp \
	         $(THERON)/EventHandler.cpp \
		 $(THERON)/NetworkEndPoint.cpp \
		 $(THERON)/Actor.cpp
THERON_OBJECTS = ${THERON_SOURCE:.cpp=.o}

# Finally we can form the full set of objective functions for the linker. 
# Currently this is only the set of Theron++ files (which should be combined 
# into a library if more utility classes are added)

ALL_MODULES = $(THERON_OBJECTS)

# Build the individual modules
#
# Compile targets

%.o : %.cpp
	$(CC) $(CFLAGS) $< -o $@ $(INCLUDE_DIRECTORIES)

# The clean target removes all object files to recompile it all

clean:
	${RM} *.o
	$(RM) ${THERON}/*.o
	${RM} *.d
	$(RM) ${THERON}/*.d

#
# TARGETS Linking the modules
#

HelloWorld : HelloWorld.o $(THERON_OBJECTS)
	$(CC) HelloWorld.o $(ALL_MODULES) $(LDFLAGS) -o HelloWorld

#
# DEPENDENCIES
#

-include $(ALL_MODULES:.o=.d)

