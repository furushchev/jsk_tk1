cmake_minimum_required(VERSION 2.8.3)
project(jsk_perception)

find_package(catkin REQUIRED COMPONENTS
  # angles
  # cmake_modules
  # cv_bridge
  dynamic_reconfigure
  geometry_msgs
  # image_geometry
  # image_transport
  # image_view2
  jsk_data
  jsk_recognition_msgs
  # jsk_recognition_utils
  jsk_topic_tools
  message_generation
  # mk
  # nodelet
  opencv_apps
  pcl_ros
  posedetection_msgs
  roscpp
  roseus
  rospack
  sensor_msgs
  std_msgs
  tf)

# ------------------------------------------------------------------------------------
# Download
# ------------------------------------------------------------------------------------

# download and install sample data
add_custom_target(install_sample_data ALL COMMAND ${PROJECT_SOURCE_DIR}/scripts/install_sample_data.py)
# download and install trained data
add_custom_target(install_trained_data ALL COMMAND ${PROJECT_SOURCE_DIR}/scripts/install_trained_data.py)

# ------------------------------------------------------------------------------------
# Catkin setup
# ------------------------------------------------------------------------------------

# Dynamic reconfigure support
generate_dynamic_reconfigure_options(
  cfg/ConcaveHullMaskImage.cfg
  cfg/RectArrayActualSizeFilter.cfg
  cfg/PolygonArrayColorLikelihood.cfg
  cfg/PolygonArrayColorHistogram.cfg
  cfg/TabletopColorDifferenceLikelihood.cfg
  cfg/ProjectImagePoint.cfg
  cfg/MaskImageGenerator.cfg
  cfg/MorphologicalMaskImageOperator.cfg
  cfg/BlobDetector.cfg
  cfg/SingleChannelHistogram.cfg
  cfg/ColorHistogramLabelMatch.cfg
  cfg/GridLabel.cfg
  cfg/SLICSuperPixels.cfg
  cfg/SnakeSegmentation.cfg
  cfg/camshiftdemo.cfg
  cfg/matchtemplate.cfg
  cfg/point_pose_extractor.cfg
  cfg/RectangleDetector.cfg
  cfg/ColorHistogram.cfg
  cfg/ColorHistogramSlidingMatcher.cfg
  cfg/BackgroundSubstraction.cfg
  cfg/GrabCut.cfg
  cfg/Fisheye.cfg
  cfg/SlidingWindowObjectDetector.cfg
  cfg/ImagePublisher.cfg
  cfg/ExtractImageChannel.cfg
  cfg/GaussianBlur.cfg
  cfg/KMeans.cfg
  cfg/ImageTimeDiff.cfg
  cfg/OverlayImageColorOnMono.cfg
  cfg/SelectiveSearch.cfg
  cfg/LabelToMaskImage.cfg
  cfg/FlowVelocityThresholding.cfg
  cfg/FilterMaskImageWithSize.cfg
)

message(WARNING "DEPRECATION WARNING: srv files in jsk_pcl_ros are deprecated."
        " Please use those in jsk_recognition_msgs.")
# TODO(wkentaro): Remove deprecated srv files.
add_service_files(FILES
  EuclideanSegment.srv
  NonMaximumSuppression.srv
  SetTemplate.srv
  WhiteBalancePoints.srv
  WhiteBalance.srv
)

generate_messages(
  DEPENDENCIES std_msgs sensor_msgs geometry_msgs jsk_recognition_msgs
)

catkin_package(
  CATKIN_DEPENDS
  geometry_msgs
  jsk_recognition_msgs
  jsk_recognition_utils
  message_runtime
  posedetection_msgs
  sensor_msgs
  std_msgs
)
