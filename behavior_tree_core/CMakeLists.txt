cmake_minimum_required(VERSION 2.8.3)
project(behavior_tree_core)



find_package(catkin REQUIRED COMPONENTS
  actionlib
  actionlib_msgs
  message_generation
  roscpp
  rospy
  std_msgs
  roslaunch
  genmsg

)


add_action_files(
  DIRECTORY action
  FILES BT.action
)


generate_messages(
  DEPENDENCIES actionlib_msgs std_msgs # Or other packages containing msgs
)


add_definitions(-Wall -lglut -lGL)

catkin_package(
  CATKIN_DEPENDS actionlib_msgs
  INCLUDE_DIRS include
)


#########################################################
# FIND GLUT
#########################################################
find_package(GLUT REQUIRED)
include_directories(${GLUT_INCLUDE_DIRS})
link_directories(${GLUT_LIBRARY_DIRS})
add_definitions(${GLUT_DEFINITIONS})
if(NOT GLUT_FOUND)
    message(ERROR " GLUT not found!")
endif(NOT GLUT_FOUND)

#########################################################
# FIND OPENGL
#########################################################
find_package(OpenGL REQUIRED)
include_directories(${OpenGL_INCLUDE_DIRS})
link_directories(${OpenGL_LIBRARY_DIRS})
add_definitions(${OpenGL_DEFINITIONS})
if(NOT OPENGL_FOUND)
    message(ERROR " OPENGL not found!")
endif(NOT OPENGL_FOUND)

INCLUDE_DIRECTORIES(${catkin_INCLUDE_DIRS} include)

file(GLOB_RECURSE BTHeadLibrary include/*.h)

set(BTSrcLibrary
src/ActionNode.cpp
src/BehaviorTree.cpp
src/ConditionNode.cpp
src/ControlNode.cpp
src/DecoratorRetryNode.cpp
src/DecoratorNegationNode.cpp
src/Draw.cpp
src/Exceptions.cpp
src/LeafNode.cpp
src/NodeSemaphore.cpp
src/ParallelNode.cpp
src/SelectorNode.cpp
src/SequenceNode.cpp
src/SelectorStarNode.cpp
src/SequenceStarNode.cpp
src/TreeNode.cpp
src/Actions/ActionTestNode.cpp
src/Conditions/ConditionTestNode.cpp
src/Actions/ROSAction.cpp
src/Conditions/ROSCondition.cpp
)


add_executable(test src/test.cpp ${BTSrcLibrary} ${BTHeadLibrary})
target_link_libraries(test ${catkin_LIBRARIES} ${OPENGL_LIBRARIES} ${GLUT_LIBRARY})
add_dependencies(test behavior_tree_core_generate_messages_cpp)


add_executable(tree src/tree.cpp ${BTSrcLibrary} ${BTHeadLibrary})
target_link_libraries(tree ${catkin_LIBRARIES} ${OPENGL_LIBRARIES} ${GLUT_LIBRARY})
add_dependencies(tree behavior_tree_core_generate_messages_cpp)


