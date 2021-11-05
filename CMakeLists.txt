cmake_minimum_required(VERSION 3.1)

project(panda_mj_description VERSION 1.0.0)

if(NOT TARGET mc_mujoco)
  find_package(mc_mujoco REQUIRED)
endif()

option(SRC_MODE "Use files in the repository instead of installed files" OFF)

set(PANDA_XML_MODEL_PATH "franka_sim/franka_panda.xml")
set(PANDA_PDGAINS_PATH "pdgains/PDgains_sim.dat")

if(SRC_MODE)
  set(PANDA_XML_MODEL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/${PANDA_XML_MODEL_PATH}")
  set(PANDA_PDGAINS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/${PANDA_PDGAINS_PATH}")
else()
  set(PANDA_XML_MODEL_PATH "${MC_MUJOCO_SHARE_DESTINATION}/panda/${PANDA_XML_MODEL_PATH}")
  set(PANDA_PDGAINS_PATH "${MC_MUJOCO_SHARE_DESTINATION}/panda/${PANDA_PDGAINS_PATH}")
endif()

configure_file(mc_mujoco/panda_hand.in.yaml "${CMAKE_CURRENT_BINARY_DIR}/mc_mujoco/panda_hand.yaml")
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/mc_mujoco/panda_hand.yaml" DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}")

install(DIRECTORY franka_sim DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}/panda" FILES_MATCHING PATTERN "*.stl")
install(DIRECTORY franka_sim DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}/panda" FILES_MATCHING PATTERN "*.xml")
install(DIRECTORY pdgains DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}/panda" FILES_MATCHING PATTERN "*.dat")
install(DIRECTORY franka_sim DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}/panda" FILES_MATCHING PATTERN "*.xml")
