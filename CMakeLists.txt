# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

project(PhysX)
set(CMAKE_INSTALL_PREFIX  ${CMAKE_CURRENT_SOURCE_DIR})
# message("${CMAKE_INSTALL_PREFIX} ${CMAKE_CURRENT_SOURCE_DIR}")

#这里四个是设置源码路径的，你需要根据你的工程目录结构来修改一下
set(PhysX .)

# INCLUDE_DIR指向physx的include文件夹，同理
set(INCLUDE_DIR ${PhysX}/physx/include)

#这里physx原先的工程里应该是叫shared_include，我改了名字
set(SHARED_INCLUDE_DIR ${PhysX}/pxshared)

#这里是physx的源文件文件夹
set(SOURCE_DIR ${PhysX}/physx/source)

#这里设置了打出来的包必定是release包 要出debug包得把${CMAKE_CXX_FLAGS_RELEASE} 修改成${CMAKE_CXX_FLAGS_DEBUG} 或者 ${CMAKE_CXX_FLAGS}
#后边的选项是包括优化等级，warning不等于error 等等
# -Wno-error -Wno-invalid-offsetof -Wno-invalid-noreturn 
set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS_RELEASE} -O3 -fshort-wchar -ffast-math -fomit-frame-pointer")
#链接器选项，不用太在意
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wall -v -Wl,--no-fatal-warnings")

#以下是physx一些相应的宏开关

#是否启用ARM_NEON
# add_definitions("-D__ARM_NEON__")

#这个不用管
add_definitions("-DPX_PHYSX_STATIC_LIB")

#平台，为啥要写俩我也忘了。。
# add_definitions("-DANDROID")
# add_definitions("-DPX_ANDROID")

#使用cooking
add_definitions("-DPX_COOKING")
#不使用pvd
add_definitions("-DPX_SUPPORT_PVD=0")

#非debug版本，如果要打debug版本则改为 add_definitions("-D _DEBUG")
add_definitions("-DNDEBUG")

#checked版本，在源码中体现为是否添加npwritecheck这个类等等，若是要打debug版本，这里必须要
add_definitions("-DPX_CHECKED")

IF(WIN32)
#do something related to WIN32
set(PLATFORM_RELATIVE_INCLUDE 
	${SHARED_INCLUDE_DIR}/include/foundation/windows
)	
set(PLATFORM_RELATIVE_SOURCE
	${SOURCE_DIR}/foundation/src/windows/PsUWPThread.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsAtomic.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsCpu.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsFPU.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsMutex.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsPrintString.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsSList.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsSocket.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsSync.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsThread.cpp
	${SOURCE_DIR}/foundation/src/windows/PsWindowsTime.cpp
)
ELSEIF(UNIX)
#do something related to UNIX
set(PLATFORM_RELATIVE_INCLUDE 
	${SHARED_INCLUDE_DIR}/include/foundation/unix
	${SOURCE_DIR}/foundation/include/unix
	${SOURCE_DIR}/foundation/include/unix/neon
	${SOURCE_DIR}/foundation/include/unix/sse2
)
set(PLATFORM_RELATIVE_SOURCE
	${SOURCE_DIR}/foundation/src/unix/PsUnixAtomic.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixCpu.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixFPU.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixMutex.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixPrintString.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixSocket.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixSync.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixThread.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixTime.cpp
	${SOURCE_DIR}/foundation/src/unix/PsUnixSList.cpp
)
ELSEIF(APPLE)
#do something related to APPLE
ENDIF(WIN32)

#头文件目录，注意我因为是安卓所以引入了很多unix的目录，你得看情况是否改成windows
include_directories(
		${PLATFORM_RELATIVE_INCLUDE}
		${INCLUDE_DIR}/vehicle
		${INCLUDE_DIR}/characterkinematic
		${SOURCE_DIR}/physxvehicle/src
		${SOURCE_DIR}/physxvehicle/src/physxmetadata/include
		${SOURCE_DIR}/common/include
		${SOURCE_DIR}/common/src
		${SOURCE_DIR}/geomUtils/include
		${SOURCE_DIR}/geomUtils/src
		${SOURCE_DIR}/geomUtils/src/ccd
		${SOURCE_DIR}/geomUtils/src/common
		${SOURCE_DIR}/geomUtils/src/contact
		${SOURCE_DIR}/geomUtils/src/convex
		${SOURCE_DIR}/geomUtils/src/distance
		${SOURCE_DIR}/geomUtils/src/gjk
		${SOURCE_DIR}/geomUtils/src/hf
		${SOURCE_DIR}/geomUtils/src/intersection
		${SOURCE_DIR}/geomUtils/src/mesh
		${SOURCE_DIR}/geomUtils/src/pcm
		${SOURCE_DIR}/geomUtils/src/sweep
		${SOURCE_DIR}/scenequery/include
		${SOURCE_DIR}/scenequery/src
		${INCLUDE_DIR}/common
        ${SHARED_INCLUDE_DIR}/include
        ${SHARED_INCLUDE_DIR}/include/foundation
        ${INCLUDE_DIR}
        ${INCLUDE_DIR}/collision
        ${INCLUDE_DIR}/cooking
        ${INCLUDE_DIR}/extensions
        ${INCLUDE_DIR}/filebuf
        ${INCLUDE_DIR}/foundation
        ${INCLUDE_DIR}/geometry
        ${INCLUDE_DIR}/solver
        ${INCLUDE_DIR}/task
        ${SOURCE_DIR}/compiler/resource_x64
        ${SOURCE_DIR}/compiler/resource_x86
        ${SOURCE_DIR}/filebuf/include
        ${SOURCE_DIR}/foundation/include
		${SOURCE_DIR}/lowlevelaabb/include
		${SOURCE_DIR}/lowlevelaabb/src
        ${SOURCE_DIR}/lowleveldynamics/include
        ${SOURCE_DIR}/lowleveldynamics/src
        ${SOURCE_DIR}/physx/src
        ${SOURCE_DIR}/physx/src/buffering
        ${SOURCE_DIR}/physx/src/device
        ${SOURCE_DIR}/physxcharacterkinematic/src
        ${SOURCE_DIR}/physxcooking/src
        ${SOURCE_DIR}/physxcooking/src/convex
        ${SOURCE_DIR}/physxcooking/src/mesh
        ${SOURCE_DIR}/physxgpu/include
        ${SOURCE_DIR}/physxmetadata/core/include
        ${SOURCE_DIR}/physxmetadata/extensions/include
        ${INCLUDE_DIR}/pvd
        ${SOURCE_DIR}/pvd/include
        ${SOURCE_DIR}/pvd/src
        ${PhysX}/tools/physxmetadatagenerator
        ${SOURCE_DIR}/physxextensions/src
		${SOURCE_DIR}/physxextensions/src/serialization
		${SOURCE_DIR}/physxextensions/src/serialization/Binary
		${SOURCE_DIR}/physxextensions/src/serialization/File
		${SOURCE_DIR}/physxextensions/src/serialization/Xml
		${SOURCE_DIR}/fastxml/include
		${SOURCE_DIR}/simulationcontroller/include
		${SOURCE_DIR}/simulationcontroller/src
		${SOURCE_DIR}/lowlevel/api/include
		${SOURCE_DIR}/lowlevel/common/include
		${SOURCE_DIR}/lowlevel/common/include/collision
		${SOURCE_DIR}/lowlevel/common/include/pipeline
		${SOURCE_DIR}/lowlevel/common/include/utils
		${SOURCE_DIR}/lowlevel/software/include
)

#源文件目录 同源文件，unix的地方可能需要改成windows
set(PHYSX_SOURCE
		${PLATFORM_RELATIVE_SOURCE}
        ${SOURCE_DIR}/physx/src/buffering/ScbScenePvdClient.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtPvd.cpp
        ${SOURCE_DIR}/physx/src/PvdMetaDataPvdBinding.cpp
        ${SOURCE_DIR}/physx/src/PvdPhysicsClient.cpp
        ${SOURCE_DIR}/physx/src/NpPvdSceneQueryCollector.cpp
        ${SOURCE_DIR}/foundation/src/PsAssert.cpp
        ${SOURCE_DIR}/foundation/src/PsAllocator.cpp
        ${SOURCE_DIR}/foundation/src/PsFoundation.cpp
        ${SOURCE_DIR}/foundation/src/PsMathUtils.cpp
        ${SOURCE_DIR}/foundation/src/PsString.cpp
        ${SOURCE_DIR}/foundation/src/PsTempAllocator.cpp
        ${SOURCE_DIR}/foundation/src/PsUtilities.cpp
        ${SOURCE_DIR}/immediatemode/src/NpImmediateMode.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpBroadPhase.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpAABBManager.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpBroadPhaseMBP.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpBroadPhaseABP.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpBroadPhaseSap.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpBroadPhaseSapAux.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpBroadPhaseShared.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpMBPTasks.cpp
        ${SOURCE_DIR}/lowlevelaabb/src/BpSAPTasks.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyArticulation.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyArticulationContactPrep.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyArticulationContactPrepPF.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyArticulationHelper.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyArticulationScalar.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyArticulationSIMD.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyConstraintPartition.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyConstraintSetup.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyConstraintSetupBlock.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyContactPrep.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyContactPrep4.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyContactPrep4PF.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyContactPrepPF.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyDynamics.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyFeatherstoneArticulation.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyFeatherstoneForwardDynamic.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyFeatherstoneInverseDynamic.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyFrictionCorrelation.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyRigidBodyToSolverBody.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DySolverConstraints.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DySolverConstraintsBlock.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DySolverControl.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DySolverControlPF.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DySolverPFConstraints.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyTGSContactPrep.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DySolverPFConstraintsBlock.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyTGSContactPrepBlock.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyTGSDynamics.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyThreadContext.cpp
        ${SOURCE_DIR}/lowLeveldynamics/src/DyThresholdTable.cpp
        ${SOURCE_DIR}/physx/src/buffering/ScbActor.cpp
        ${SOURCE_DIR}/physx/src/buffering/ScbAggregate.cpp
        ${SOURCE_DIR}/physx/src/buffering/ScbBase.cpp
        ${SOURCE_DIR}/physx/src/buffering/ScbMetaData.cpp
        ${SOURCE_DIR}/physx/src/buffering/ScbScene.cpp
        ${SOURCE_DIR}/physx/src/buffering/ScbShape.cpp
        ${SOURCE_DIR}/physx/src/device/linux/PhysXIndicatorLinux.cpp
        ${SOURCE_DIR}/physx/src/NpActor.cpp
        ${SOURCE_DIR}/physx/src/NpAggregate.cpp
        ${SOURCE_DIR}/physx/src/NpArticulation.cpp
        ${SOURCE_DIR}/physx/src/NpArticulationJoint.cpp
        ${SOURCE_DIR}/physx/src/NpArticulationJointReducedCoordinate.cpp
        ${SOURCE_DIR}/physx/src/NpArticulationReducedCoordinate.cpp
        ${SOURCE_DIR}/physx/src/NpArticulationLink.cpp
        ${SOURCE_DIR}/physx/src/NpBatchQuery.cpp
        ${SOURCE_DIR}/physx/src/NpConstraint.cpp
        ${SOURCE_DIR}/physx/src/NpFactory.cpp
        ${SOURCE_DIR}/physx/src/NpMaterial.cpp
        ${SOURCE_DIR}/physx/src/NpMetaData.cpp
        ${SOURCE_DIR}/physx/src/NpPhysics.cpp
        ${SOURCE_DIR}/physx/src/NpReadCheck.cpp
        ${SOURCE_DIR}/physx/src/NpRigidDynamic.cpp
        ${SOURCE_DIR}/physx/src/NpRigidStatic.cpp
        ${SOURCE_DIR}/physx/src/NpScene.cpp
        ${SOURCE_DIR}/physx/src/NpSceneQueries.cpp
        ${SOURCE_DIR}/physx/src/NpSerializerAdapter.cpp
        ${SOURCE_DIR}/physx/src/NpShape.cpp
        ${SOURCE_DIR}/physx/src/NpShapeManager.cpp
        ${SOURCE_DIR}/physx/src/NpWriteCheck.cpp
        ${SOURCE_DIR}/physxcooking/src/Adjacencies.cpp
        ${SOURCE_DIR}/physxcooking/src/BVHStructureBuilder.cpp
        ${SOURCE_DIR}/physxcooking/src/Cooking.cpp
        ${SOURCE_DIR}/physxcooking/src/CookingUtils.cpp
        ${SOURCE_DIR}/physxcooking/src/EdgeList.cpp
        ${SOURCE_DIR}/physxcooking/src/MeshCleaner.cpp
        ${SOURCE_DIR}/physxcooking/src/Quantizer.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/BigConvexDataBuilder.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/ConvexHullBuilder.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/ConvexHullLib.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/ConvexHullUtils.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/ConvexMeshBuilder.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/ConvexPolygonsBuilder.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/QuickHullConvexHullLib.cpp
        ${SOURCE_DIR}/physxcooking/src/convex/VolumeIntegration.cpp
        ${SOURCE_DIR}/physxcooking/src/mesh/GrbTriangleMeshCooking.cpp
        ${SOURCE_DIR}/physxcooking/src/mesh/HeightFieldCooking.cpp
        ${SOURCE_DIR}/physxcooking/src/mesh/MeshBuilder.cpp
        ${SOURCE_DIR}/physxcooking/src/mesh/RTreeCooking.cpp
        ${SOURCE_DIR}/physxcooking/src/mesh/TriangleMeshBuilder.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtBroadPhase.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtCollection.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtConvexMeshExt.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtContactJoint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtCpuWorkerThread.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtD6Joint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtD6JointCreate.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtDefaultCpuDispatcher.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtDefaultErrorCallback.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtDefaultSimulationFilterShader.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtDefaultStreams.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtDistanceJoint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtExtensions.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtFixedJoint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtJoint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtMetaData.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtPrismaticJoint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtPxStringTable.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtRaycastCCD.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtRevoluteJoint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtRigidBodyExt.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtRigidActorExt.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtSceneQueryExt.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtSimpleFactory.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtSmoothNormals.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtSphericalJoint.cpp
        ${SOURCE_DIR}/physxextensions/src/ExtTriangleMeshExt.cpp
        ${SOURCE_DIR}/physxmetadata/core/src/PxAutoGeneratedMetaDataObjects.cpp
        ${SOURCE_DIR}/physxmetadata/core/src/PxMetaDataObjects.cpp
        ${SOURCE_DIR}/physxmetadata/extensions/src/PxExtensionAutoGeneratedMetaDataObjects.cpp
        ${SOURCE_DIR}/simulationcontroller/src/ScActorCore.cpp
        ${SOURCE_DIR}/task/src/TaskManager.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnBinaryDeserialization.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnBinarySerialization.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnConvX_Align.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnConvX_Convert.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnConvX_Error.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnConvX_MetaData.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnConvX_Output.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnConvX_Union.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnConvX.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Binary/SnSerializationContext.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Xml/SnJointRepXSerializer.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Xml/SnRepXCoreSerializer.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Xml/SnRepXUpgrader.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/Xml/SnXmlSerialization.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/SnSerialization.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/SnSerialUtils.cpp
		${SOURCE_DIR}/physxextensions/src/serialization/SnSerializationRegistry.cpp
		${SOURCE_DIR}/fastxml/src/PsFastXml.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScActorSim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScArticulationCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScArticulationJointCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScArticulationJointSim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScArticulationSim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScBodyCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScBodySim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScConstraintCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScConstraintGroupNode.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScConstraintInteraction.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScConstraintProjectionManager.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScConstraintProjectionTree.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScConstraintSim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScElementInteractionMarker.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScElementSim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScInteraction.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScIterators.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScMetaData.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScNPhaseCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScPhysics.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScRigidCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScRigidSim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScScene.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScShapeCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScShapeInteraction.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScShapeSim.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScSimStats.cpp
		${SOURCE_DIR}/simulationcontroller/src/Scsimulationcontroller.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScSqBoundsManager.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScStaticCore.cpp
		${SOURCE_DIR}/simulationcontroller/src/ScTriggerInteraction.cpp
		${SOURCE_DIR}/scenequery/src/SqAABBPruner.cpp
		${SOURCE_DIR}/scenequery/src/SqAABBTree.cpp
		${SOURCE_DIR}/scenequery/src/SqAABBTreeUpdateMap.cpp
		${SOURCE_DIR}/scenequery/src/SqBounds.cpp
		${SOURCE_DIR}/scenequery/src/SqBucketPruner.cpp
		${SOURCE_DIR}/scenequery/src/SqCompoundPruner.cpp
		${SOURCE_DIR}/scenequery/src/SqCompoundPruningPool.cpp
		${SOURCE_DIR}/scenequery/src/SqExtendedBucketPruner.cpp
		${SOURCE_DIR}/scenequery/src/SqIncrementalAABBPruner.cpp
		${SOURCE_DIR}/scenequery/src/SqIncrementalAABBPrunerCore.cpp
		${SOURCE_DIR}/scenequery/src/SqIncrementalAABBTree.cpp
		${SOURCE_DIR}/scenequery/src/SqMetaData.cpp
		${SOURCE_DIR}/scenequery/src/SqPruningPool.cpp
		${SOURCE_DIR}/scenequery/src/SqPruningStructure.cpp
		${SOURCE_DIR}/scenequery/src/SqSceneQueryManager.cpp
		${SOURCE_DIR}/lowlevel/api/src/px_globals.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcContactCache.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcContactMethodImpl.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcMaterialHeightField.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcMaterialMesh.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcMaterialMethodImpl.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcMaterialShape.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcNpBatch.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcNpCacheStreamPair.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcNpContactPrepShared.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcNpMemBlockPool.cpp
		${SOURCE_DIR}/lowlevel/common/src/pipeline/PxcNpThreadContext.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsCCD.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsContactManager.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsContext.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsDefaultMemoryManager.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsIslandSim.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsMaterialCombiner.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsNphaseImplementationContext.cpp
		${SOURCE_DIR}/lowlevel/software/src/PxsSimpleIslandManager.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctBoxController.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctCapsuleController.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctCharacterController.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctCharacterControllerCallbacks.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctCharacterControllerManager.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctController.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctObstacleContext.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctSweptBox.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctSweptCapsule.cpp
		${SOURCE_DIR}/physxcharacterkinematic/src/CctSweptVolume.cpp
		${SOURCE_DIR}/physxvehicle/src/physxmetadata/src/PxVehicleAutoGeneratedMetaDataObjects.cpp
		${SOURCE_DIR}/physxvehicle/src/physxmetadata/src/PxVehicleMetaDataObjects.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleComponents.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleDrive.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleDrive4W.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleDriveNW.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleDriveTank.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleNoDrive.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleSDK.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleSerialization.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleSuspWheelTire4.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleTireFriction.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleUpdate.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleWheels.cpp
		${SOURCE_DIR}/physxvehicle/src/VehicleUtilControl.cpp
		${SOURCE_DIR}/physxvehicle/src/VehicleUtilSetup.cpp
		${SOURCE_DIR}/physxvehicle/src/VehicleUtilTelemetry.cpp
		${SOURCE_DIR}/physxvehicle/src/PxVehicleMetaData.cpp
		${SOURCE_DIR}/common/src/CmCollection.cpp
		${SOURCE_DIR}/common/src/CmMathUtils.cpp
		${SOURCE_DIR}/common/src/CmPtrTable.cpp
		${SOURCE_DIR}/common/src/CmRadixSort.cpp
		${SOURCE_DIR}/common/src/CmRadixSortBuffered.cpp
		${SOURCE_DIR}/common/src/CmRenderOutput.cpp
		${SOURCE_DIR}/common/src/CmVisualization.cpp
		${SOURCE_DIR}/geomutils/src/ccd/GuCCDSweepConvexMesh.cpp
		${SOURCE_DIR}/geomutils/src/ccd/GuCCDSweepPrimitives.cpp
		${SOURCE_DIR}/geomutils/src/common/GuBarycentricCoordinates.cpp
		${SOURCE_DIR}/geomutils/src/common/GuSeparatingAxes.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactBoxBox.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactCapsuleBox.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactCapsuleCapsule.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactCapsuleConvex.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactCapsuleMesh.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactConvexConvex.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactConvexMesh.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactPlaneBox.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactPlaneCapsule.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactPlaneConvex.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactPolygonPolygon.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactSphereBox.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactSphereCapsule.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactSphereMesh.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactSpherePlane.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuContactSphereSphere.cpp
		${SOURCE_DIR}/geomutils/src/contact/GuFeatureCode.cpp
		${SOURCE_DIR}/geomutils/src/convex/GuBigConvexData.cpp
		${SOURCE_DIR}/geomutils/src/convex/GuConvexHelper.cpp
		${SOURCE_DIR}/geomutils/src/convex/GuConvexMesh.cpp
		${SOURCE_DIR}/geomutils/src/convex/GuConvexSupportTable.cpp
		${SOURCE_DIR}/geomutils/src/convex/GuConvexUtilsInternal.cpp
		${SOURCE_DIR}/geomutils/src/convex/GuHillClimbing.cpp
		${SOURCE_DIR}/geomutils/src/convex/GuShapeConvex.cpp
		${SOURCE_DIR}/geomutils/src/distance/GuDistancePointBox.cpp
		${SOURCE_DIR}/geomutils/src/distance/GuDistancePointTriangle.cpp
		${SOURCE_DIR}/geomutils/src/distance/GuDistanceSegmentBox.cpp
		${SOURCE_DIR}/geomutils/src/distance/GuDistanceSegmentSegment.cpp
		${SOURCE_DIR}/geomutils/src/distance/GuDistanceSegmentTriangle.cpp
		${SOURCE_DIR}/geomutils/src/gjk/GuEPA.cpp
		${SOURCE_DIR}/geomutils/src/gjk/GuGJKSimplex.cpp
		${SOURCE_DIR}/geomutils/src/gjk/GuGJKTest.cpp
		${SOURCE_DIR}/geomutils/src/GuBounds.cpp
		${SOURCE_DIR}/geomutils/src/GuAABBTreeBuild.cpp
		${SOURCE_DIR}/geomutils/src/GuBVHStructure.cpp
		${SOURCE_DIR}/geomutils/src/GuBox.cpp
		${SOURCE_DIR}/geomutils/src/GuCapsule.cpp
		${SOURCE_DIR}/geomutils/src/GuCCTSweepTests.cpp
		${SOURCE_DIR}/geomutils/src/GuGeometryQuery.cpp
		${SOURCE_DIR}/geomutils/src/GuGeometryUnion.cpp
		${SOURCE_DIR}/geomutils/src/GuInternal.cpp
		${SOURCE_DIR}/geomutils/src/GuMeshFactory.cpp
		${SOURCE_DIR}/geomutils/src/GuMetaData.cpp
		${SOURCE_DIR}/geomutils/src/GuMTD.cpp
		${SOURCE_DIR}/geomutils/src/GuOverlapTests.cpp
		${SOURCE_DIR}/geomutils/src/GuRaycastTests.cpp
		${SOURCE_DIR}/geomutils/src/GuSerialize.cpp
		${SOURCE_DIR}/geomutils/src/GuSweepMTD.cpp
		${SOURCE_DIR}/geomutils/src/GuSweepSharedTests.cpp
		${SOURCE_DIR}/geomutils/src/GuSweepTests.cpp
		${SOURCE_DIR}/geomutils/src/hf/GuHeightField.cpp
		${SOURCE_DIR}/geomutils/src/hf/GuHeightFieldUtil.cpp
		${SOURCE_DIR}/geomutils/src/hf/GuOverlapTestsHF.cpp
		${SOURCE_DIR}/geomutils/src/hf/GuSweepsHF.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionBoxBox.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionCapsuleTriangle.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionEdgeEdge.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionRayBox.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionRayCapsule.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionRaySphere.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionSphereBox.cpp
		${SOURCE_DIR}/geomutils/src/intersection/GuIntersectionTriangleBox.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV32.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV32Build.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4Build.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_AABBSweep.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_BoxOverlap.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_CapsuleSweep.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_CapsuleSweepAA.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_OBBSweep.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_Raycast.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_SphereOverlap.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuBV4_SphereSweep.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuMeshQuery.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuMidphaseBV4.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuMidphaseRTree.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuOverlapTestsMesh.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuRTree.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuRTreeQueries.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuSweepsMesh.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuTriangleMesh.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuTriangleMeshBV4.cpp
		${SOURCE_DIR}/geomutils/src/mesh/GuTriangleMeshRTree.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactBoxBox.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactBoxConvex.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactCapsuleBox.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactCapsuleCapsule.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactCapsuleConvex.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactCapsuleHeightField.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactCapsuleMesh.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactConvexCommon.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactConvexConvex.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactConvexHeightField.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactConvexMesh.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactGenBoxConvex.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactGenSphereCapsule.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactPlaneBox.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactPlaneCapsule.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactPlaneConvex.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactSphereBox.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactSphereCapsule.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactSphereConvex.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactSphereHeightField.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactSphereMesh.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactSpherePlane.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMContactSphereSphere.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMShapeConvex.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPCMTriangleContactGen.cpp
		${SOURCE_DIR}/geomutils/src/pcm/GuPersistentContactManifold.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepBoxBox.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepBoxSphere.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepBoxTriangle_FeatureBased.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepBoxTriangle_SAT.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepCapsuleBox.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepCapsuleCapsule.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepCapsuleTriangle.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepSphereCapsule.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepSphereSphere.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepSphereTriangle.cpp
		${SOURCE_DIR}/geomutils/src/sweep/GuSweepTriangleUtils.cpp
)

#这里是find(这里只是找到)了一些对于安卓平台的依赖库，windows平台也许有对应的依赖（因为我手头没有windows的电脑所以没编过，不知道到底需要啥依赖😂）
# include(AndroidNdkModules)
# android_ndk_import_module_cpufeatures()

#生成库文件，打包出来应该是叫 libPhysx.lib
add_library( # Sets the name of the library.
             PhysX

             # Sets the library as a shared library.
             STATIC

             # Provides a relative path to your source file(s).
			${PHYSX_SOURCE}
         )

#设置链接器语言，不用管
set_target_properties(PhysX PROPERTIES LINKER_LANGUAGE CXX)

# # Searches for a specified prebuilt library and stores the path as a
# # variable. Because CMake includes system libraries in the search path by
# # default, you only need to specify the name of the public NDK library
# # you want to add. CMake verifies that the library exists before
# # completing its build.


# # Specifies libraries CMake should link to your target library. You
# # can link multiple libraries, such as libraries you define in this
# # build script, prebuilt third-party libraries, or system libraries.

# #这里是安卓才有的依赖，用于打印日志的，windows大概率不需要
# find_library( # Sets the name of the path variable.
# 		log-lib

# 		# Specifies the name of the NDK library that
# 		# you want CMake to locate.
# 		log  )

#链接库。这里才是让对应的库参与到physx的链接过程中
# target_link_libraries( # Specifies the target library.
#                         PhysX

#                         # Links the target library to the log library
#                         # included in the NDK.
#                         ${log-lib} cpufeatures)