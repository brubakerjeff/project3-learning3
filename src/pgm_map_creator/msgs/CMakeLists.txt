find_package(Protobuf REQUIRED)
message(STATUS "My variable is: ${PROTOBUF_LIBRARY}")
message(STATUS "My variable is: ${PROTOBUF_INCLUDE_DIRS}")

set(PROTOBUF_IMPORT_DIRS)
foreach(ITR ${GAZEBO_INCLUDE_DIRS})
  if(ITR MATCHES ".*gazebo-[0-9.]+$")
    set(PROTOBUF_IMPORT_DIRS "${ITR}/gazebo/msgs/proto")
  endif()
endforeach()

set (msgs
  collision_map_request.proto
  #${PROTOBUF_IMPORT_DIRS}/vector2d.proto
  #${PROTOBUF_IMPORT_DIRS}/header.proto
  #${PROTOBUF_IMPORT_DIRS}/time.proto
)
PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS ${msgs})
add_library(collision_map_creator_msgs SHARED ${PROTO_SRCS})
target_include_directories(collision_map_creator_msgs PUBLIC ${PROTOBUF_INCLUDE_DIRS} /usr/include/gazebo-7/gazebo/msgs/)

target_link_libraries(collision_map_creator_msgs ${PROTOBUF_LIBRARY} )
