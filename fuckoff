local whitelist = {
	1786942767, 
    7866522358, 
	11099363994  
}

local kickReason = "You are not whitelisted to use this script. contact @alonebychoice if u wish to buy"


local player = game.Players.LocalPlayer
if not table.find(whitelist, player.UserId) then
	player:Kick(kickReason)
	
	return
end

getgenv().config = {
    dist = 5,
    sets = {
        scale = 8.5,
        delay = 0.0000000000005,
        pingbased = false
    },
    exp_v = {
        velbased = true,
        nostutter = true
    }
}

-- Services & Globals
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")

local fti = firetouchinterest
local V3 = Vector3.new
local V3zero = Vector3.zero
local abs, clamp, sqrt, max, min, floor = math.abs, math.clamp, math.sqrt, math.max, math.min, math.floor
local clock = os.clock
local taskWait = task.wait
local taskSpawn = task.spawn
local taskDelay = task.delay

-- Constants from first script
local BALL_NAMES = {TPS=true, Ball=true, TPSClientBall_Proxy=true}
local WELD_TYPES = {Weld=true, WeldConstraint=true, Motor6D=true, RigidConstraint=true, BallSocketConstraint=true, HingeConstraint=true, UniversalConstraint=true, Snap=true, NoCollisionConstraint=true}
local FIRST_NUKE_DB = 0.0005
local INSIDE_NUKE_DB = 0.0008
local CHAIN_BOOST_AT = 25
local CHAIN_BOOST_R = 0.012
local CHAIN_NORM = 0.0032
local CHAIN_FAST = 0.00018
local STICKY_F = 5.2
local BASE_AIR    = {X=2, Z=2, Y=2}
local GROUND_REACH= 2
local INSIDE_RSQ = 1.69

-- Constants from second script
local HEADER_POWER = 12
local INSIDE_THRESH = 1.3
local INSIDE_CD = 0.12
local DRIBBLE_CD = 0.016
local DRIBBLE_DIST_SQ = 25
local HIGH_PING_MS = 0.07
local GRAV = -98.1
local STEP = 1/60
local INV60 = 1/60
local GRAV_HALF_SQ = 0.5 * GRAV * (STEP * STEP)

-- ===== FLAG CONFIGURATIONS (KEPT FROM ORIGINAL) =====
-- CONFIG 1: For HEADERS, AIR LEGS, VOLLEYS, AIR HEAD (Aggressive - ZERO DELAY)
local AERIAL_FLAGS = {
    ["DFIntTargetTimeDelayFacctorTenths"] = "0",
    ["FIntInterpolationMaxDelayMSec"] = "0",
    ["DFIntMaxFrameBufferSize"] = "0",
    ["DFIntS2PhysicsSenderRate"] = "3768900",
    ["DFIntTargetTimeDelay"] = "0",
    ["DFIntPhysicsSenderRate"] = "0",
    ["FIntClientReplayLengthMs"] = "0",
    ["DFIntClientBulletReplicationDelay"] = "0",
    ["DFIntMoveSmootherTimeDelayClient"] = "0",
    ["DFIntMoveSmootherTimeDelayServer"] = "0",
    ["FIntLagReductionMaxDelayMs"] = "0",
    ["DFIntReplicationSimulatedLagMs"] = "0",
    ["DFIntReplicationLatencyMs"] = "0",
    ["DFIntReplicationLagMs"] = "0",
    ["DFIntPacketSendFrequency"] = "60",
    ["DFIntMaxUpdateRateHz"] = "60",
    ["DFIntHeartbeatSendRate"] = "60",
    ["DFIntMaxPacketSize"] = "1024",
    ["DFIntThrottledVelocityThresholdTenths"] = "15",
    ["DFIntTangentialVelocityCoefficientHundredths"] = "100",
    ["DFIntMaxLocomotionForce"] = "1000000",
    ["DFIntCodecMaxOutgoingFrames"] = "10000",
    ["DFIntMaxProcessPacketsStepsPerCyclic"] = "50000",
    ["DFIntLargePacketQueueSizeCutoffMB"] = "1000",
    ["DFIntMaxProcessPacketsJobScaling"] = "10000",
    ["FIntSimWorldTaskQueueParallelTasks"] = "1",
    ["DFIntMaxProcessPacketsStepsAccumulated"] = "0",
    ["DFIntVelocityAwareStreamingThreshold2"] = "100",
    ["DFIntGameNetPVHeaderRotationOrientIdToleranceExponent"] = "-2147483647",
    ["DFIntCodecMaxIncomingPackets"] = "10000",
    ["DFIntWaitOnRecvFromLoopEndedMS"] = "100",
    ["DFIntGameNetPVHeaderTranslationZeroCutoffExponent"] = "-2147483647",
    ["DFIntGameNetOptimizeParallelPhysicsSendAssemblyBatch"] = "2147483647",
    ["FFlagHandleFrameSize1Velocity"] = "True",
    ["DFIntSimDesiredVelocityDiffCutoff"] = "50",
    ["DFIntGameNetPVHeaderLinearVelocityZeroCutoffExponent"] = "-2147483647",
    ["DFIntGameNetPVHeaderRotationalVelocityZeroCutoffExponent"] = "-2147483647",
    ["DFFlagPhysicsSkipNonRealTimeHumanoidForceCalc2"] = "False",
    ["DFIntMaxAltitudePDStickHipHeightPercent"] = "8",
    ["DFFlagDebugPauseVoxelizer"] = "True",
    ["FFlagUseUncappedWalkSpeedClient"] = "True",
    ["DFIntOptimizePingThreshold"] = "10",
    ["FFlagInputSendTimeWithEvent"] = "False",
    ["FFlagEnableFrameByFrameMovement"] = "True",
    ["FFlagKeyboardEventBypassThrottle"] = "True",
    ["FIntInputThrottleMS"] = "0",
    ["FFlagInputLatencyCompensationV2"] = "True",
    ["DFIntInputFrameDelay"] = "0",
    ["FFlagEnableClientPredictionPosition"] = "True",
    ["FFlagEnableMovementEarlySend"] = "True",
    ["FFlagKeyboardInputFastPath"] = "True",
    ["FIntActivatedCountTimerMSKeyboard"] = "0",
    ["FIntActivatedCountTimerMSMouse"] = "0",
    ["DFIntPerformanceControlFrameTimeMax"] = "1",
    ["DFIntPerformanceControlFrameTimeMaxUtility"] = "-1",
    ["DFFlagSimSolverImprovedPartialSleepDefaultEnabled"] = "False",
    ["DFFlagEnablePerfDataSubsystemTimersCollection2"] = "False",
    ["DFFlagIkControlConstraintSupportDefaultEnabled"] = "True",
    ["DFFlagPerformanceControlEnableMemoryProbing3"] = "True",
    ["DFFlagPlayerHumanoidPropertyUpdateRestrict"] = "False",
    ["DFFlagSimAdaptiveUseNewVelocityCriteria"] = "False",
    ["DFFlagPhysicsMechanismCacheOptimizeAlloc"] = "true",
    ["DFFlagSkipReadDiskCacheRedirects"] = "true",
    ["DFFlagSimAdaptiveEnable60HzParts"] = "True",
    ["DFFlagEnableControlRigIkTargets"] = "True",
    ["DFFlagSampleAndRefreshRakPing"] = "true",
    ["DFIntCheckPVLinearVelocityIntegrateVsDeltaPositionThresholdPercent"] = "0",
    ["DFIntConnectionLostDisconnectReasonInfluxHundredthsPercentage"] = "0",
    ["DFIntBandwidthManagerFeedbackInfluxHundredthsPercentage"] = "100",
    ["DFIntSimAdaptiveHumanoidPDControllerSubstepMultiplier"] = "32",
    ["DFIntGroundControllerBalancingResponsiveness"] = "2147000000",
    ["DFIntSimLevitationAirModeFakeTurningImpulse"] = "1147483647",
    ["DFIntInterpolationFrameRotVelocityThresholdMillionth"] = "5",
    ["DFIntGroundControllerTurningResponsiveness"] = "2147000000",
    ["DFIntThrottlingPredictionAccelerationHoldThousandth"] = "2",
    ["DFIntJoinDataItemEstimatedCompressionRatioHundreths"] = "0",
    ["DFIntAirControllerBalancingResponsiveness"] = "2147000000",
    ["DFIntPhysicsEMAInverseSmoothingFactorThrottling"] = "128",
    ["DFIntInterpolationFramePositionThresholdMillionth"] = "5",
    ["DFIntInterpolationFrameVelocityThresholdMillionth"] = "5",
    ["DFIntAirControllerTurningResponsiveness"] = "2147000000",
    ["DFIntGameNetCompressionLodByteBudgetThresholdPct"] = "0",
    ["DFIntSimSolverImprovedPartialSleepAngularWeight"] = "0",
    ["DFIntCoreScriptsAnalyticsHundredthsPercentage"] = "0",
    ["DFIntSimSolverStatisticsHundrethsPercentage"] = "100",
    ["DFIntSimSolverImprovedPartialSleepTolerance"] = "0",
    ["DFIntTimeBetweenSendConnectionAttemptsMS"] = "200",
    ["DFIntMaxInterpolationRecursionsBeforeCheck"] = "1",
    ["DFIntSimConstraintDataCollectionRate3"] = "36420",
    ["DFIntVisibilityCheckRayCastLimitPerFrame"] = "10",
    ["DFIntStreamJobNOUVolumeLengthCap"] = "2147483647",
    ["DFIntWorldStepsOffsetAdjustRate"] = "2147483648",
    ["DFIntSmoothTerrainPhysicsRayAabbSlop"] = "-9999",
    ["DFIntDownloadEpisodeTimerFreqeuncyInMs"] = "100",
    ["DFIntInterpolationNumMechanismsBatchSize"] = "1",
    ["DFIntPhysicsEMAInverseSmoothingFactor"] = "512",
    ["DFIntMinClientSimulationRadius"] = "2147000000",
    ["DFIntNumFramesToKeepAfterInterpolation"] = "1",
    ["DFIntPhysicsNOUCountHundredth"] = "2147483647",
    ["DFIntSimSlidingVelocitySleepThreshold"] = "0",
    ["DFIntSimSolverImprovedSleepCriteriaH"] = "30",
    ["DFIntServerBandwidthPlayerSampleRate"] = "15",
    ["DFIntMaxAverageFrameDelayExceedFactor"] = "2",
    ["DFIntCachedPatchLoadDelayMilliseconds"] = "0",
    ["DFIntRagdollEarlyExitTimeTenthSecond"] = "1",
    ["DFIntBatchPostExpirationTimeSeconds"] = "10",
    ["DFIntCountCoalescedTouchesHundredth"] = "-1",
    ["DFIntRCCMergeSameTouchesTimeoutInMS"] = "-1",
    ["DFIntMinimalSimRadiusBuffer"] = "2147000000",
    ["DFIntMaxAltitudePDHipHeightPercent"] = "4",
    ["DFIntTaskSchedulerJobInGameThreads"] = "11",
    ["DFIntStreamJobNOUVolumeCap"] = "2147483647",
    ["DFIntPhysicsReceiveNumParallelTasks"] = "6",
    ["DFIntFrameRateMSToReduceTouchEvents"] = "0",
    ["DFIntSkipSomePropertiesPermyriad"] = "5000",
    ["DFIntRagdollDefaultTimerTenthSecond"] = "1",
    ["DFIntInterpolationMinAssemblyCount"] = "1",
    ["DFIntTaskSchedulerJobInitThreads"] = "11",
    ["DFIntRCCTouchBucketTimeSizeInMS"] = "1",
    ["DFIntBufferCompressionThreshold"] = "100",
    ["DFIntSimArbiterCFLLengthScaleThou"] = "1",
    ["DFIntLocalThrottleCallbackDelayMs"] = "0",
    ["DFIntInterpolationDtLimitForLod"] = "10",
    ["DFIntRagdollSlowEnoughToNotTenth"] = "1",
    ["DFIntSimAdaptiveExtraIterations"] = "16",
    ["DFIntUnstickForceAttackInTenths"] = "-4",
    ["DFIntBatchPostJobRunsPerSecond"] = "500",
    ["DFIntSimBroadPhasePairCountMax"] = "50",
    ["DFIntRemoteCursorUpdateDelayMs"] = "0",
    ["DFIntCanHideGuiGroupId"] = "32380007",
    ["DFIntJoinDataCompressionLevel"] = "0",
    ["DFIntMaxAcceptableUpdateDelay"] = "1",
    ["DFIntServerFramesBetweenJoins"] = "1",
    ["DFIntConnectingTimerInterval"] = "10",
    ["DFIntRobustRetargeterIkMethod"] = "4",
    ["DFIntOcclusionDensityRatio"] = "20",
    ["DFIntAngularVelociryLimit"] = "360",
    ["DFIntBufferCompressionLevel"] = "0",
    ["DFIntRingingTimerInterval"] = "10",
    ["DFIntWarpFactor"] = "2147483648",
    ["DFIntRuntimeConcurrency"] = "11",
    ["DFIntDataSenderRate"] = "36420",
    ["FIntOptimizedContactPipelineSpatialHashOverlapToleranceMicro"] = "5000",
    ["FIntFastClusterHumanoidWorstCaseGeometryDataBytes"] = "5242880",
    ["FIntInterpolationAwareTargetTimeLerpHundredth"] = "100",
    ["FIntSmoothMouseSpringFrequencyTenths"] = "100",
    ["FIntSimSolverResponsiveness"] = "2147000000",
    ["FIntIkDraggerSolveIterations"] = "2048",
    ["FIntLuaGcParallelMinMultiTasks"] = "11",
    ["FIntLDLStasisCachePurgeRate"] = "1200",
    ["FIntActivatedCountTimerMSTouch"] = "1",
    ["FIntAICOInputDebounceTimeMs"] = "0",
    ["FIntCursorRetryDelayMs"] = "0",
    ["FIntCLI20390_2"] = "0",
    ["FStringDeathTypeValue"] = "Ragdoll",
    ["DFIntMemoryUtilityCurveBaseHundrethsPercent"] = "2500",
    ["DFIntMemoryUtilityCurveNumSegments"] = "0",
    ["FFlagOptimizeCFrameUpdates4"] = "True",
    ["FFlagOptimizeCFrameUpdatesIC5"] = "True",
    ["FFlagCreationDBCompressRequest"] = "False",
    ["DFIntClusterEstimatedCompressionRatioHundredths"] = "0",
    ["DFIntSendGameServerDataMaxLen"] = "9999999",
    ["DFIntClusterCompressionLevel"] = "0",
    ["FIntDebugForceMSAASamples"] = "1",
    ["FFlagHandleAltEnterFullscreenManually"] = "False",
    ["FFlagGameBasicSettingsFramerateCap5"] = "False",
    ["FIntMaxSimulatedPlayers"] = "50",
    ["FFlagSimLinearVelocitySpanningTreeFix2"] = "True",
    ["FFlagEnableGetMoveVelocity2"] = "True",
    ["FIntAllowBufferedUpdates"] = "True",
    ["FIntPacketSendRateHz"] = "60",
    ["DFFlagDebugRenderForceTechnologyVoxel"] = "True",
    ["FIntInterpolationMaxTimeMS"] = "50",
    ["FFlagDisableFeedbackSoothsayerCheck"] = "False",
    ["DFFlagSimSetAssemblyVelocityBeforeInWorld"] = "True",
    ["FlntTerrainArraySliceSize"] = "O",
    ["FlntMaxSimulatedPlayers"] = "50",
    ["FFlagAdServiceEnabled"] = "False",
    ["DFIntPhysicsSenderRate"] = "264",
    ["FIntInterpolationAwareTargetTimeLerpHundredth"] = "256"
}

-- CONFIG 2: For NORMAL gameplay (Standard values)
local NORMAL_FLAGS = {
    ["FFlagHandleAltEnterFullscreenManually"] = "False",
    ["FFlagGameBasicSettingsFramerateCap5"] = "False",
    ["FIntMaxSimulatedPlayers"] = "50",
    ["DFIntPacketSendFrequency"] = "60",
    ["DFIntMaxUpdateRateHz"] = "60",
    ["FFlagSimLinearVelocitySpanningTreeFix2"] = "True",
    ["FFlagEnableGetMoveVelocity2"] = "True",
    ["FIntAllowBufferedUpdates"] = "True",
    ["FIntPacketSendRateHz"] = "60",
    ["DFFlagDebugRenderForceTechnologyVoxel"] = "True",
    ["FIntInterpolationMaxTimeMS"] = "50",
    ["DFIntHeartbeatSendRate"] = "60",
    ["DFIntTargetTimeDelayFacctorTenths"] = "9",
    ["DFIntMaxPacketSize"] = "1024",
    ["FFlagDisableFeedbackSoothsayerCheck"] = "False",
    ["FIntInterpolationMaxDelayMSec"] = "60",
    ["DFIntThrottledVelocityThresholdTenths"] = "15",
    ["DFIntTangentialVelocityCoefficientHundredths"] = "100",
    ["DFFlagSimSetAssemblyVelocityBeforeInWorld"] = "True",
    ["DFIntMaxLocomotionForce"] = "1000000",
    ["DFIntCodecMaxOutgoingFrames"] = "10000",
    ["DFIntMaxProcessPacketsStepsPerCyclic"] = "50000",
    ["DFIntLargePacketQueueSizeCutoffMB"] = "1000",
    ["DFIntMaxProcessPacketsJobScaling"] = "10000",
    ["FIntSimWorldTaskQueueParallelTasks"] = "1",
    ["DFIntMaxProcessPacketsStepsAccumulated"] = "0",
    ["DFIntVelocityAwareStreamingThreshold2"] = "100",
    ["DFIntGameNetPVHeaderRotationOrientIdToleranceExponent"] = "-2147483647",
    ["DFIntCodecMaxIncomingPackets"] = "10000",
    ["DFIntWaitOnRecvFromLoopEndedMS"] = "100",
    ["DFIntGameNetPVHeaderTranslationZeroCutoffExponent"] = "-2147483647",
    ["DFIntGameNetOptimizeParallelPhysicsSendAssemblyBatch"] = "2147483647",
    ["FFlagHandleFrameSize1Velocity"] = "True",
    ["DFIntSimDesiredVelocityDiffCutoff"] = "50",
    ["DFIntGameNetPVHeaderLinearVelocityZeroCutoffExponent"] = "-2147483647",
    ["DFIntGameNetPVHeaderRotationalVelocityZeroCutoffExponent"] = "-2147483647",
    ["DFFlagPhysicsSkipNonRealTimeHumanoidForceCalc2"] = "False",
    ["DFIntMaxAltitudePDStickHipHeightPercent"] = "8",
    ["DFFlagDebugPauseVoxelizer"] = "True",
    ["FFlagUseUncappedWalkSpeedClient"] = "True",
    ["DFIntOptimizePingThreshold"] = "10",
    ["FFlagInputSendTimeWithEvent"] = "False",
    ["FFlagEnableFrameByFrameMovement"] = "True",
    ["FFlagKeyboardEventBypassThrottle"] = "True",
    ["FIntInputThrottleMS"] = "0",
    ["FFlagInputLatencyCompensationV2"] = "True",
    ["DFIntInputFrameDelay"] = "0",
    ["FFlagEnableClientPredictionPosition"] = "True",
    ["FFlagEnableMovementEarlySend"] = "True",
    ["FFlagKeyboardInputFastPath"] = "True",
    ["FlntTerrainArraySliceSize"] = "O",
    ["FlntMaxSimulatedPlayers"] = "50",
    ["FFlagAdServiceEnabled"] = "False",
    ["FIntActivatedCountTimerMSKeyboard"] = "0",
    ["FIntActivatedCountTimerMSMouse"] = "0",
    ["DFIntPerformanceControlFrameTimeMax"] = "1",
    ["DFIntPerformanceControlFrameTimeMaxUtility"] = "-1",
    ["DFIntMaxFrameBufferSize"] = "4",
    ["DFFlagSimSolverImprovedPartialSleepDefaultEnabled"] = "False",
    ["DFFlagEnablePerfDataSubsystemTimersCollection2"] = "False",
    ["DFFlagIkControlConstraintSupportDefaultEnabled"] = "True",
    ["DFFlagPerformanceControlEnableMemoryProbing3"] = "True",
    ["DFFlagPlayerHumanoidPropertyUpdateRestrict"] = "False",
    ["DFFlagSimAdaptiveUseNewVelocityCriteria"] = "False",
    ["DFFlagPhysicsMechanismCacheOptimizeAlloc"] = "true",
    ["DFFlagSkipReadDiskCacheRedirects"] = "true",
    ["DFFlagSimAdaptiveEnable60HzParts"] = "True",
    ["DFFlagEnableControlRigIkTargets"] = "True",
    ["DFFlagSampleAndRefreshRakPing"] = "true",
    ["DFIntCheckPVLinearVelocityIntegrateVsDeltaPositionThresholdPercent"] = "0",
    ["DFIntConnectionLostDisconnectReasonInfluxHundredthsPercentage"] = "0",
    ["DFIntBandwidthManagerFeedbackInfluxHundredthsPercentage"] = "100",
    ["DFIntSimAdaptiveHumanoidPDControllerSubstepMultiplier"] = "32",
    ["DFIntGroundControllerBalancingResponsiveness"] = "2147000000",
    ["DFIntSimLevitationAirModeFakeTurningImpulse"] = "1147483647",
    ["DFIntInterpolationFrameRotVelocityThresholdMillionth"] = "5",
    ["DFIntGroundControllerTurningResponsiveness"] = "2147000000",
    ["DFIntThrottlingPredictionAccelerationHoldThousandth"] = "2",
    ["DFIntJoinDataItemEstimatedCompressionRatioHundreths"] = "0",
    ["DFIntAirControllerBalancingResponsiveness"] = "2147000000",
    ["DFIntPhysicsEMAInverseSmoothingFactorThrottling"] = "128",
    ["DFIntInterpolationFramePositionThresholdMillionth"] = "5",
    ["DFIntInterpolationFrameVelocityThresholdMillionth"] = "5",
    ["DFIntAirControllerTurningResponsiveness"] = "2147000000",
    ["DFIntGameNetCompressionLodByteBudgetThresholdPct"] = "0",
    ["DFIntSimSolverImprovedPartialSleepAngularWeight"] = "0",
    ["DFIntCoreScriptsAnalyticsHundredthsPercentage"] = "0",
    ["DFIntSimSolverStatisticsHundrethsPercentage"] = "100",
    ["DFIntSimSolverImprovedPartialSleepTolerance"] = "0",
    ["DFIntTimeBetweenSendConnectionAttemptsMS"] = "200",
    ["DFIntMaxInterpolationRecursionsBeforeCheck"] = "1",
    ["DFIntSimConstraintDataCollectionRate3"] = "36420",
    ["DFIntVisibilityCheckRayCastLimitPerFrame"] = "10",
    ["DFIntStreamJobNOUVolumeLengthCap"] = "2147483647",
    ["DFIntWorldStepsOffsetAdjustRate"] = "2147483648",
    ["DFIntSmoothTerrainPhysicsRayAabbSlop"] = "-9999",
    ["DFIntDownloadEpisodeTimerFreqeuncyInMs"] = "100",
    ["DFIntInterpolationNumMechanismsBatchSize"] = "1",
    ["DFIntPhysicsEMAInverseSmoothingFactor"] = "512",
    ["DFIntMinClientSimulationRadius"] = "2147000000",
    ["DFIntNumFramesToKeepAfterInterpolation"] = "1",
    ["DFIntPhysicsNOUCountHundredth"] = "2147483647",
    ["DFIntSimSlidingVelocitySleepThreshold"] = "0",
    ["DFIntSimSolverImprovedSleepCriteriaH"] = "30",
    ["DFIntServerBandwidthPlayerSampleRate"] = "15",
    ["DFIntMaxAverageFrameDelayExceedFactor"] = "2",
    ["DFIntCachedPatchLoadDelayMilliseconds"] = "0",
    ["DFIntRagdollEarlyExitTimeTenthSecond"] = "1",
    ["DFIntBatchPostExpirationTimeSeconds"] = "10",
    ["DFIntCountCoalescedTouchesHundredth"] = "-1",
    ["DFIntRCCMergeSameTouchesTimeoutInMS"] = "-1",
    ["DFIntMinimalSimRadiusBuffer"] = "2147000000",
    ["DFIntMaxAltitudePDHipHeightPercent"] = "4",
    ["DFIntTaskSchedulerJobInGameThreads"] = "11",
    ["DFIntStreamJobNOUVolumeCap"] = "2147483647",
    ["DFIntPhysicsReceiveNumParallelTasks"] = "6",
    ["DFIntFrameRateMSToReduceTouchEvents"] = "0",
    ["DFIntSkipSomePropertiesPermyriad"] = "5000",
    ["DFIntRagdollDefaultTimerTenthSecond"] = "1",
    ["DFIntInterpolationMinAssemblyCount"] = "1",
    ["DFIntTaskSchedulerJobInitThreads"] = "11",
    ["DFIntRCCTouchBucketTimeSizeInMS"] = "1",
    ["DFIntBufferCompressionThreshold"] = "100",
    ["DFIntSimArbiterCFLLengthScaleThou"] = "1",
    ["DFIntLocalThrottleCallbackDelayMs"] = "0",
    ["DFIntInterpolationDtLimitForLod"] = "10",
    ["DFIntRagdollSlowEnoughToNotTenth"] = "1",
    ["DFIntSimAdaptiveExtraIterations"] = "16",
    ["DFIntUnstickForceAttackInTenths"] = "-4",
    ["DFIntBatchPostJobRunsPerSecond"] = "500",
    ["DFIntSimBroadPhasePairCountMax"] = "50",
    ["DFIntRemoteCursorUpdateDelayMs"] = "0",
    ["DFIntCanHideGuiGroupId"] = "32380007",
    ["DFIntJoinDataCompressionLevel"] = "0",
    ["DFIntMaxAcceptableUpdateDelay"] = "1",
    ["DFIntServerFramesBetweenJoins"] = "1",
    ["DFIntConnectingTimerInterval"] = "10",
    ["DFIntRobustRetargeterIkMethod"] = "4",
    ["DFIntOcclusionDensityRatio"] = "20",
    ["DFIntAngularVelociryLimit"] = "360",
    ["DFIntBufferCompressionLevel"] = "0",
    ["DFIntRingingTimerInterval"] = "10",
    ["DFIntPhysicsSenderRate"] = "264",
    ["DFIntWarpFactor"] = "2147483648",
    ["DFIntRuntimeConcurrency"] = "11",
    ["DFIntDataSenderRate"] = "36420",
    ["FIntOptimizedContactPipelineSpatialHashOverlapToleranceMicro"] = "5000",
    ["FIntFastClusterHumanoidWorstCaseGeometryDataBytes"] = "5242880",
    ["FIntInterpolationAwareTargetTimeLerpHundredth"] = "100",
    ["FIntSmoothMouseSpringFrequencyTenths"] = "100",
    ["FIntSimSolverResponsiveness"] = "2147000000",
    ["FIntIkDraggerSolveIterations"] = "2048",
    ["FIntLuaGcParallelMinMultiTasks"] = "11",
    ["FIntLDLStasisCachePurgeRate"] = "1200",
    ["FIntActivatedCountTimerMSTouch"] = "1",
    ["FIntAICOInputDebounceTimeMs"] = "0",
    ["FIntCursorRetryDelayMs"] = "0",
    ["FIntCLI20390_2"] = "0",
    ["FStringDeathTypeValue"] = "Ragdoll",
    ["DFIntMemoryUtilityCurveBaseHundrethsPercent"] = "2500",
    ["DFIntMemoryUtilityCurveNumSegments"] = "0",
    ["FFlagOptimizeCFrameUpdates4"] = "True",
    ["FFlagOptimizeCFrameUpdatesIC5"] = "True",
    ["FFlagCreationDBCompressRequest"] = "False",
    ["DFIntClusterEstimatedCompressionRatioHundredths"] = "0",
    ["DFIntSendGameServerDataMaxLen"] = "9999999",
    ["DFIntClusterCompressionLevel"] = "0",
    ["FIntDebugForceMSAASamples"] = "1"
}

-- Flag manipulation helpers
local function stripFlagPrefix(name)
    return name:gsub("^DFInt",""):gsub("^DFFlag",""):gsub("FString","")
                :gsub("FLog",""):gsub("^FFlag",""):gsub("^DFint",""):gsub("^FInt","")
end

local flag_targetTimeDelay = stripFlagPrefix("DFIntTargetTimeDelayFacctorTenths")
local flag_targetTimeDelayRaw = "DFIntTargetTimeDelayFacctorTenths"
local flag_interpolationMaxDelay = stripFlagPrefix("FIntInterpolationMaxDelayMSec")
local flag_interpolationMaxDelayRaw = "FIntInterpolationMaxDelayMSec"
local flag_maxFrameBuffer = stripFlagPrefix("DFIntMaxFrameBufferSize")
local flag_maxFrameBufferRaw = "DFIntMaxFrameBufferSize"
local flag_senderRate = stripFlagPrefix("DFIntS2PhysicsSenderRate")
local flag_senderRateRaw = "DFIntS2PhysicsSenderRate"

local lastWrite = {t="", i="", fb="", sr=""}

-- ===== OPTIMIZED FLAG APPLICATION (NO LAG) =====
local currentFlagValues = {}

local function applyFlagConfigOptimized(config)
    if not setfflag then return end
    for flagName, value in pairs(config) do
        if currentFlagValues[flagName] ~= value then
            currentFlagValues[flagName] = value
            pcall(function()
                if getfflag(flagName) ~= nil then
                    setfflag(flagName, value)
                end
            end)
        end
    end
end

-- Apply normal flags at start
applyFlagConfigOptimized(NORMAL_FLAGS)

local function writeFlags(timeDelayValue, interpValue, frameBufferValue, senderRateValue)
    if not setfflag then return end
    if timeDelayValue ~= lastWrite.t or interpValue ~= lastWrite.i then
        lastWrite.t = timeDelayValue
        lastWrite.i = interpValue
        pcall(function()
            if getfflag(flag_targetTimeDelay) ~= nil then
                setfflag(flag_targetTimeDelay, timeDelayValue)
            elseif getfflag(flag_targetTimeDelayRaw) ~= nil then
                setfflag(flag_targetTimeDelayRaw, timeDelayValue)
            end
        end)
        pcall(function()
            if getfflag(flag_interpolationMaxDelay) ~= nil then
                setfflag(flag_interpolationMaxDelay, interpValue)
            elseif getfflag(flag_interpolationMaxDelayRaw) ~= nil then
                setfflag(flag_interpolationMaxDelayRaw, interpValue)
            end
        end)
    end
    if frameBufferValue ~= lastWrite.fb then
        lastWrite.fb = frameBufferValue
        pcall(function()
            if getfflag(flag_maxFrameBuffer) ~= nil then
                setfflag(flag_maxFrameBuffer, frameBufferValue)
            elseif getfflag(flag_maxFrameBufferRaw) ~= nil then
                setfflag(flag_maxFrameBufferRaw, frameBufferValue)
            end
        end)
    end
    if senderRateValue ~= lastWrite.sr then
        lastWrite.sr = senderRateValue
        pcall(function()
            if getfflag(flag_senderRate) ~= nil then
                setfflag(flag_senderRate, senderRateValue)
            elseif getfflag(flag_senderRateRaw) ~= nil then
                setfflag(flag_senderRateRaw, senderRateValue)
            end
        end)
    end
end

local function writeNormal()
    writeFlags("9", "60", lastWrite.fb, lastWrite.sr)
end

-- For AERIAL actions - apply aggressive flags (OPTIMIZED)
local function writeAerialFlags()
    applyFlagConfigOptimized(AERIAL_FLAGS)
end

local function writeRestore()
    lastWrite.t = ""
    lastWrite.i = ""
    lastWrite.fb = ""
    lastWrite.sr = ""
    writeFlags("9", "60", "0", "3768900")
    pcall(function()
        if not setfflag then return end
        if getfflag(flag_maxFrameBuffer) ~= nil then
            setfflag(flag_maxFrameBuffer, "4")
        elseif getfflag(flag_maxFrameBufferRaw) ~= nil then
            setfflag(flag_maxFrameBufferRaw, "4")
        end
        if getfflag(flag_senderRate) ~= nil then
            setfflag(flag_senderRate, "15")
        elseif getfflag(flag_senderRateRaw) ~= nil then
            setfflag(flag_senderRateRaw, "15")
        end
    end)
    lastWrite.fb = "4"
    lastWrite.sr = "15"
end

local flagRestoreThreads = {nil, nil}
local flagActive = {false, false}

-- Trigger aerial flags
local function triggerContactFlag(idx)
    if flagRestoreThreads[idx] then
        pcall(task.cancel, flagRestoreThreads[idx])
        flagRestoreThreads[idx] = nil
    end
    flagActive[idx] = true
    writeAerialFlags()
    flagRestoreThreads[idx] = task.delay(0.1, function()
        flagActive[idx] = false
        if not flagActive[1] and not flagActive[2] then
            applyFlagConfigOptimized(NORMAL_FLAGS)
        end
    end)
end

-- Initialize normal flags
if setfflag then
    pcall(function()
        if getfflag(flag_targetTimeDelay) ~= nil then
            setfflag(flag_targetTimeDelay, "9")
        elseif getfflag(flag_targetTimeDelayRaw) ~= nil then
            setfflag(flag_targetTimeDelayRaw, "9")
        end
    end)
    pcall(function()
        if getfflag(flag_interpolationMaxDelay) ~= nil then
            setfflag(flag_interpolationMaxDelay, "60")
        elseif getfflag(flag_interpolationMaxDelayRaw) ~= nil then
            setfflag(flag_interpolationMaxDelayRaw, "60")
        end
    end)
    lastWrite.t = "9"
    lastWrite.i = "60"
    lastWrite.fb = "4"
    lastWrite.sr = "15"
end

-- Remove suspicious remotes (anti-cheat)
local function destroyRemotePath(path)
    local parts = {}
    for part in string.gmatch(path, "[^/]+") do
        table.insert(parts, part)
    end
    local current = ReplicatedStorage
    for _, part in ipairs(parts) do
        current = current:FindFirstChild(part)
        if not current then return false end
    end
    if current then
        current:Destroy()
        return true
    end
    return false
end

local function isSuspiciousRemote(name)
    return string.match(name, "[%W_]+") ~= nil
end

local remoteCleaned = false
local remotePaths = {
    "Remotes/RemoteEvents/IlllIIIllIIIIIlIIlIIIlI/Exploit",
    "Remotes/RemoteEvents/IlllIIIllIIIIIlIIlIIIlI/Bindable/Exploit"
}
for _, path in ipairs(remotePaths) do
    if destroyRemotePath(path) then remoteCleaned = true end
end
if not remoteCleaned then
    for _, child in pairs(ReplicatedStorage:GetChildren()) do
        if child:IsA("RemoteEvent") and isSuspiciousRemote(child.Name) then
            child:Destroy()
            remoteCleaned = true
        end
    end
end

-- Helper functions for character manipulation
local function removeWeldFromPart(character, partName)
    local model = Workspace:FindFirstChild(character.Name)
    if model then
        local part = model:FindFirstChild(partName)
        if part then
            local weld = part:FindFirstChild("Weld")
            if weld then weld:Destroy() end
        end
    end
end

local function disableCollisions(character)
    local model = Workspace:FindFirstChild(character.Name)
    if model then
        for _, part in pairs(model:GetChildren()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end

RunService.Heartbeat:Connect(function()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            removeWeldFromPart(player.Character, "Legs")
            removeWeldFromPart(player.Character, "Head")
            removeWeldFromPart(player.Character, "Header")
            disableCollisions(player.Character)
        end
    end
end)

pcall(function() setfpscap(999) end)

-- ===== IMPROVED WELD NUKING SYSTEM - WIN ALL CLASHES =====
local _allBalls = {}
local _ballConns = {}
local _debWeld = {}
local _insideLast = {}
local _insideNow = {}
local _chainCount = {}
local _chainLastT = {}
local _nukeT = {}
local _nukeDeepT = {}

-- ULTRA AGGRESSIVE WELD DESTRUCTION - Scans entire hierarchy
local function ultraNukeWelds(ball, depth)
    depth = depth or 0
    if depth > 5 then return end
    
    -- Destroy ALL welds on the ball itself
    for _, o in ipairs(ball:GetChildren()) do
        if WELD_TYPES[o.ClassName] then 
            pcall(function() o:Destroy() end)
        end
        -- Also destroy any constraints
        if o:IsA("Constraint") then
            pcall(function() o:Destroy() end)
        end
    end
    
    -- Check parent for welds
    local par = ball.Parent
    if par then
        for _, o in ipairs(par:GetChildren()) do
            if WELD_TYPES[o.ClassName] then 
                local ok0, p0 = pcall(function() return o.Part0 end)
                local ok1, p1 = pcall(function() return o.Part1 end)
                if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                    pcall(function() o:Destroy() end)
                end
            end
            if o:IsA("Constraint") then
                local ok0, p0 = pcall(function() return o.Part0 end)
                local ok1, p1 = pcall(function() return o.Part1 end)
                if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                    pcall(function() o:Destroy() end)
                end
            end
        end
        
        -- Check parent's parent
        local par2 = par.Parent
        if par2 then
            for _, o in ipairs(par2:GetChildren()) do
                if WELD_TYPES[o.ClassName] then 
                    local ok0, p0 = pcall(function() return o.Part0 end)
                    local ok1, p1 = pcall(function() return o.Part1 end)
                    if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                        pcall(function() o:Destroy() end)
                    end
                end
                if o:IsA("Constraint") then
                    local ok0, p0 = pcall(function() return o.Part0 end)
                    local ok1, p1 = pcall(function() return o.Part1 end)
                    if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                        pcall(function() o:Destroy() end)
                    end
                end
            end
        end
    end
    
    -- Also destroy any welds connected to this ball from any player
    for _, player in pairs(Players:GetPlayers()) do
        if player.Character then
            for _, part in pairs(player.Character:GetDescendants()) do
                if WELD_TYPES[part.ClassName] then
                    local ok0, p0 = pcall(function() return part.Part0 end)
                    local ok1, p1 = pcall(function() return part.Part1 end)
                    if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                        pcall(function() part:Destroy() end)
                    end
                end
                if part:IsA("Constraint") then
                    local ok0, p0 = pcall(function() return part.Part0 end)
                    local ok1, p1 = pcall(function() return part.Part1 end)
                    if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                        pcall(function() part:Destroy() end)
                    end
                end
            end
        end
    end
end

-- Enhanced weld sweeping
local function aggressiveSweepForBall(parent, ball)
    if not parent or not ball then return end
    for _, o in ipairs(parent:GetChildren()) do
        local c = o.ClassName
        if c == "Weld" or c == "WeldConstraint" or c == "Motor6D" or c == "RigidConstraint" or c == "BallSocketConstraint" or c == "HingeConstraint" or c == "UniversalConstraint" or c == "Snap" or c == "NoCollisionConstraint" then
            local ok0, p0 = pcall(function() return o.Part0 end)
            local ok1, p1 = pcall(function() return o.Part1 end)
            if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                pcall(function() o:Destroy() end)
            end
        end
        if o:IsA("Constraint") then
            local ok0, p0 = pcall(function() return o.Part0 end)
            local ok1, p1 = pcall(function() return o.Part1 end)
            if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                pcall(function() o:Destroy() end)
            end
        end
    end
end

-- Enhanced nuke functions - MUCH MORE AGGRESSIVE
local function nukeWeldsL1(ball)
    local now = clock()
    local lt = _nukeT[ball]
    if lt and now - lt < FIRST_NUKE_DB then return end
    _nukeT[ball] = now
    
    -- Destroy all welds on ball
    for _, o in ipairs(ball:GetChildren()) do
        if WELD_TYPES[o.ClassName] then 
            pcall(function() o:Destroy() end)
        end
        if o:IsA("Constraint") then
            pcall(function() o:Destroy() end)
        end
    end
    
    -- Sweep parent and grandparent
    local par = ball.Parent
    if par then 
        aggressiveSweepForBall(par, ball)
        local par2 = par.Parent
        if par2 then 
            aggressiveSweepForBall(par2, ball)
        end
    end
end

local function nukeWeldsDeep(ball)
    local now = clock()
    local lt = _nukeDeepT[ball]
    if lt and now - lt < INSIDE_NUKE_DB then return end
    _nukeDeepT[ball] = now
    
    -- Full nuclear option - destroy EVERYTHING connected to this ball
    ultraNukeWelds(ball, 0)
    
    -- Also force-unweld from any player character
    for _, player in pairs(Players:GetPlayers()) do
        if player.Character and player ~= LocalPlayer then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    -- Check for any attachments/welds to the ball
                    for _, att in pairs(part:GetChildren()) do
                        if att:IsA("Attachment") then
                            for _, constraint in pairs(att:GetChildren()) do
                                if constraint:IsA("Constraint") then
                                    local ok0, p0 = pcall(function() return constraint.Part0 end)
                                    local ok1, p1 = pcall(function() return constraint.Part1 end)
                                    if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                                        pcall(function() constraint:Destroy() end)
                                    end
                                end
                            end
                        end
                    end
                end
            end
        end
    end
end

-- CONTINUOUS WELD MONITOR - Destroys any weld that appears
local function continuousWeldMonitor(ball)
    local weldCheckConn = ball.ChildAdded:Connect(function(child)
        if WELD_TYPES[child.ClassName] or child:IsA("Constraint") then
            pcall(function() child:Destroy() end)
        end
    end)
    
    -- Also monitor parent for new welds
    local parent = ball.Parent
    if parent then
        local parentWeldConn = parent.ChildAdded:Connect(function(child)
            if WELD_TYPES[child.ClassName] or child:IsA("Constraint") then
                local ok0, p0 = pcall(function() return child.Part0 end)
                local ok1, p1 = pcall(function() return child.Part1 end)
                if (ok0 and p0 == ball) or (ok1 and p1 == ball) then
                    pcall(function() child:Destroy() end)
                end
            end
        end)
        
        if not _ballConns[ball] then _ballConns[ball] = {} end
        table.insert(_ballConns[ball], parentWeldConn)
    end
    
    if not _ballConns[ball] then _ballConns[ball] = {} end
    table.insert(_ballConns[ball], weldCheckConn)
end

-- Ball velocity/force interceptors (keep ball from being controlled by others)
local function interceptBV(ball, bv)
    local function cap() 
        pcall(function() 
            local v = bv.Velocity
            local h = v.X*v.X + v.Z*v.Z
            if h > 121 then 
                local s = 11/sqrt(h)
                bv.Velocity = V3(v.X*s, v.Y, v.Z*s)
            end
            -- Prevent other players from adding velocity to "our" ball
            if v.Magnitude > 150 then
                bv.Velocity = V3(v.X * 0.5, v.Y, v.Z * 0.5)
            end
        end) 
    end
    cap()
    local conn = bv:GetPropertyChangedSignal("Velocity"):Connect(cap)
    if not _ballConns[ball] then _ballConns[ball] = {} end
    table.insert(_ballConns[ball], conn)
end

local function interceptBF(ball, bf)
    local function cap() 
        pcall(function() 
            local f = bf.Force
            local m = f.Magnitude
            if m > 180 then 
                bf.Force = f * (180/m)
            end
            -- Reduce force from other players
            if m > 100 then
                bf.Force = f * 0.3
            end
        end) 
    end
    cap()
    local conn = bf:GetPropertyChangedSignal("Force"):Connect(cap)
    if not _ballConns[ball] then _ballConns[ball] = {} end
    table.insert(_ballConns[ball], conn)
end

local function interceptAV(ball, av)
    pcall(function() av.AngularVelocity = V3zero end)
    local conn = av:GetPropertyChangedSignal("AngularVelocity"):Connect(function() 
        pcall(function() av.AngularVelocity = V3zero end) 
    end)
    if not _ballConns[ball] then _ballConns[ball] = {} end
    table.insert(_ballConns[ball], conn)
end

local function watchBall(ball)
    -- Add continuous weld monitoring
    continuousWeldMonitor(ball)
    
    for _, c in ipairs(ball:GetChildren()) do
        if c:IsA("BodyVelocity") then interceptBV(ball, c)
        elseif c:IsA("BodyForce") then interceptBF(ball, c)
        elseif c:IsA("AngularVelocity") then interceptAV(ball, c)
        end
    end
    local childConn = ball.ChildAdded:Connect(function(c)
        if c:IsA("BodyVelocity") then interceptBV(ball, c)
        elseif c:IsA("BodyForce") then interceptBF(ball, c)
        elseif c:IsA("AngularVelocity") then interceptAV(ball, c)
        end
    end)
    if not _ballConns[ball] then _ballConns[ball] = {} end
    table.insert(_ballConns[ball], childConn)
end

local function cleanBall(ball)
    if not _allBalls[ball] then return end
    _allBalls[ball] = nil
    _debWeld[ball] = nil
    _insideLast[ball] = nil
    _insideNow[ball] = nil
    _chainCount[ball] = nil
    _chainLastT[ball] = nil
    _nukeT[ball] = nil
    _nukeDeepT[ball] = nil
    if _ballConns[ball] then
        for _, conn in ipairs(_ballConns[ball]) do
            pcall(function() conn:Disconnect() end)
        end
        _ballConns[ball] = nil
    end
end

local function addBall(ball)
    if _allBalls[ball] then return end
    _allBalls[ball] = true
    watchBall(ball)
    
    -- IMMEDIATE weld nuke on ball detection
    ultraNukeWelds(ball, 0)
    
    if ball.Name == "TPSClientBall_Proxy" then
        task.defer(function()
            pcall(function()
                local sys = workspace:FindFirstChild("TPSSystem")
                local rb = sys and sys:FindFirstChild("TPS") or workspace:FindFirstChild("Ball")
                if not rb then return end
                for _, o in ipairs(ball:GetChildren()) do
                    if o:IsA("WeldConstraint") then return end
                end
                local wc = Instance.new("WeldConstraint")
                wc.Part0 = ball
                wc.Part1 = rb
                wc.Parent = ball
            end)
        end)
    end
    
    -- TOUCH DETECTION - Nuke welds on ANY touch from opponent
    local touchConn = ball.Touched:Connect(function(hit)
        -- If hit is an opponent player's body part
        if hit.Parent and hit.Parent ~= LocalPlayer.Character and hit.Parent:FindFirstChild("Humanoid") then
            -- IMMEDIATE WELD NUKE - prevents opponent from welding
            ultraNukeWelds(ball, 0)
            nukeWeldsDeep(ball)
            
            -- Also remove any velocity they tried to apply
            pcall(function()
                ball.AssemblyLinearVelocity = ball.AssemblyLinearVelocity * 0.5
            end)
        end
        
        if hit.Parent ~= LocalPlayer.Character or not _ALLOWED[hit.Name] or _debWeld[ball] then return end
        _debWeld[ball] = true
        nukeWeldsL1(ball)
        task.delay(0.03, function() _debWeld[ball] = nil end)
    end)
    
    if not _ballConns[ball] then _ballConns[ball] = {} end
    table.insert(_ballConns[ball], touchConn)
end

local _ALLOWED = {
    Head = true, UpperTorso = true, LowerTorso = true, Torso = true,
    ["Left Leg"] = true, ["Right Leg"] = true, RightLowerLeg = true, LeftLowerLeg = true,
    RightUpperLeg = true, LeftUpperLeg = true
}

-- Track existing balls
local function trackAllBalls()
    for _, o in ipairs(workspace:GetDescendants()) do
        if o:IsA("BasePart") and BALL_NAMES[o.Name] then
            addBall(o)
        end
    end
end

trackAllBalls()
workspace.DescendantAdded:Connect(function(d)
    if d:IsA("BasePart") and BALL_NAMES[d.Name] then
        task.defer(function() addBall(d) end)
    end
end)
workspace.DescendantRemoving:Connect(function(d) cleanBall(d) end)

-- Local character references
local _character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local _rootPart, _humanoid, _head, _rightLeg, _leftLeg
local _hasRightLeg, _hasLeftLeg = false, false

local function refreshCharacter(character)
    if not character then return end
    _character = character
    _rootPart = character:FindFirstChild("HumanoidRootPart")
    _humanoid = character:FindFirstChild("Humanoid")
    _head = character:FindFirstChild("Head")
    _rightLeg = character:FindFirstChild("RightLowerLeg") or character:FindFirstChild("Right Leg")
    _leftLeg = character:FindFirstChild("LeftLowerLeg") or character:FindFirstChild("Left Leg")
    _hasRightLeg = _rightLeg ~= nil
    _hasLeftLeg = _leftLeg ~= nil
end

refreshCharacter(_character)
LocalPlayer.CharacterAdded:Connect(function(char)
    task.wait()
    _character = char
    refreshCharacter(char)
    char.ChildAdded:Connect(function() refreshCharacter(char) end)
end)

-- Ball prediction buffer
local BUF_MAX = 10
local POS_BUF = {}
for i = 1, BUF_MAX do
    POS_BUF[i] = {t=0, px=0, py=0, pz=0, vx=0, vy=0, vz=0}
end
local POS_HEAD = 0
local POS_LEN = 0

local function recordBall(ball)
    POS_HEAD = (POS_HEAD % BUF_MAX) + 1
    local s = POS_BUF[POS_HEAD]
    local p = ball.Position
    local v = ball.AssemblyLinearVelocity
    s.t = clock()
    s.px = p.X; s.py = p.Y; s.pz = p.Z
    s.vx = v.X; s.vy = v.Y; s.vz = v.Z
    if POS_LEN < BUF_MAX then POS_LEN = POS_LEN + 1 end
end

local function getFrame(offset)
    return POS_BUF[((POS_HEAD - offset - 1) % BUF_MAX) + 1]
end

local function smartPredict(ball, ahead)
    if POS_LEN < 2 then
        if not ball then return V3zero, V3zero end
        local bp = ball.Position
        local bv = ball.AssemblyLinearVelocity
        return V3(bp.X + bv.X*ahead, bp.Y + bv.Y*ahead + 0.5*GRAV*ahead*ahead, bp.Z + bv.Z*ahead), bv
    end
    local s2 = getFrame(0); local s1 = getFrame(1)
    local dt = s2.t - s1.t
    if dt <= 0 then dt = 0.016 end
    local idt = 1 / dt
    local vpx = (s2.px - s1.px) * idt
    local vpy = (s2.py - s1.py) * idt
    local vpz = (s2.pz - s1.pz) * idt
    local vrx = s2.vx; local vry = s2.vy; local vrz = s2.vz
    local ddx = vrx - vpx; local ddy = vry - vpy; local ddz = vrz - vpz
    local kb = clamp((ddx*ddx + ddy*ddy + ddz*ddz) * (1/1225), 0, 1)
    local ikb = 1 - kb
    local blx = vpx*ikb + vrx*kb
    local bly = vpy*ikb + vry*kb
    local blz = vpz*ikb + vrz*kb
    local ax, ay, az = 0, 0, 0
    if POS_LEN >= 3 then
        local s0 = getFrame(2)
        local dt0 = s1.t - s0.t
        if dt0 > 0 then
            local idt0 = 1 / dt0
            local v0x = (s1.px - s0.px) * idt0
            local v0y = (s1.py - s0.py) * idt0
            local v0z = (s1.pz - s0.pz) * idt0
            ax = clamp((vpx - v0x) * idt, -120, 120)
            ay = clamp((vpy - v0y) * idt, -120, 120)
            az = clamp((vpz - v0z) * idt, -120, 120)
        end
    end
    local gravY = 0.5 * GRAV * ahead * ahead * ikb
    local t = clamp(ahead, 0.016, 0.25)
    local half = 0.5 * t * t
    return V3(s2.px + blx*t + ax*half,
              s2.py + bly*t + ay*half + gravY,
              s2.pz + blz*t + az*half),
           V3(blx, bly, blz)
end

local function arcXYZ(bx, by, bz, vx, vy, vz, t)
    return bx + vx*t, by + vy*t + 0.5*GRAV*t*t, bz + vz*t
end

local function headerR(bvx, bvy, bvz, rvx, rvy, rvz, hd)
    if not hd or hd < 2 or bvy > -4 then return 1.8 end
    local rs = sqrt((bvx-rvx)^2 + (bvy-rvy)^2 + (bvz-rvz)^2)
    return 1.8 + clamp(-bvy*0.035, 0, 1)*0.6 + clamp(rs*0.04, 0, 1)*0.8 + clamp((hd-2)*0.15, 0, 1)*0.3
end

local function headerRY(bvy, hd)
    if not hd or hd < 2 or bvy > -4 then return 1.8 end
    return 1.8 + clamp(-bvy*0.04, 0, 1)*1.0
end

local function calcDynamicReach(ballY, vx, vy, vz, rootY)
    local heightDiff = ballY - rootY
    local latSpdSq = vx*vx + vz*vz
    local latSpd = clamp(sqrt(latSpdSq) * (1/40), 0, 1)
    local dropSpeed = clamp(-vy * (1/60), 0, 1)
    local heightBon = clamp((heightDiff - 4) * (1/8), 0, 1)
    local timeToArr = (vy < -1) and (heightDiff / -vy) or 99
    local urgency = clamp(1 - timeToArr * (1/0.3), 0, 1)
    local dynY = clamp(2 + dropSpeed*2 + heightBon + urgency, 2, 5.5)
    local zApp = clamp(math.abs(vz) * (1/20), 0, 1)
    local totalSpd = sqrt(latSpdSq + vy*vy)
    local slowPen = clamp(1 - totalSpd*(1/15), 0, 1) * 0.7
    local tooHigh = clamp((heightDiff - 5) * (1/3), 0, 1) * 1.4
    local dynZ = clamp(2 + latSpd*0.8 + zApp*0.6 - slowPen - tooHigh, 1.2, 3.2)
    local xApp = clamp(math.abs(vx) * (1/20), 0, 1)
    local dynX = clamp(2 + latSpd*0.5 + xApp*0.6 - tooHigh, 1.6, 4.2)
    return dynX*0.75, dynZ*0.75, dynY*0.75
end

local function fireLegTouch(ball)
    if not ball or not _rootPart then return end
    -- NUKE WELDS before touching to ensure no opponent weld sticks
    ultraNukeWelds(ball, 0)
    
    pcall(fti, ball, _rootPart, 0)
    pcall(fti, ball, _rootPart, 1)
    if _hasRightLeg then pcall(fti, ball, _rightLeg, 0); pcall(fti, ball, _rightLeg, 1) end
    if _hasLeftLeg then pcall(fti, ball, _leftLeg, 0); pcall(fti, ball, _leftLeg, 1) end
end

local function fireDribbleBurst(ball)
    if not ball then return end
    ultraNukeWelds(ball, 0)
    if _hasRightLeg then pcall(fti, ball, _rightLeg, 0); pcall(fti, ball, _rightLeg, 1) end
    if _hasLeftLeg then pcall(fti, ball, _leftLeg, 0); pcall(fti, ball, _leftLeg, 1) end
    pcall(fti, ball, _rootPart, 0); pcall(fti, ball, _rootPart, 1)
end

local function fireInsideBurst(ball)
    if not ball then return end
    ultraNukeWelds(ball, 0)
    for i = 1, 3 do
        fireLegTouch(ball)
    end
end

local function fireDribble(ball, bx, bz, vx, vz, rx, rz, lx, lz, ping, bvy, bspdH, spinY, now)
    local dx = bx - rx
    local dz = bz - rz
    local dist = sqrt(dx*dx + dz*dz)
    local inv = 1 / math.max(dist, 0.001)
    local nx = dx * inv
    local nz = dz * inv
    local driftOut = vx*nx + vz*nz
    local lookDot = lx*nx + lz*nz
    local spinPull = spinY * dist * 0.028
    local stickyK = clamp(1 + lookDot*0.7 + clamp(-bvy*0.036, 0, 1)*0.5, 1, 2.4)
    local cc = _chainCount[ball] or 0
    local chainBonus = clamp(cc - CHAIN_BOOST_AT + 1, 0, 3) * CHAIN_BOOST_R
    local approachBonus = clamp(-driftOut*0.04, 0, 0.8)
    local reachR = GROUND_REACH + ping*bspdH*0.95 + clamp(bspdH*0.026, 0, 1)*0.6 + chainBonus + approachBonus
    if dist <= reachR then
        -- NUKE WELDS before ANY touch during clash
        ultraNukeWelds(ball, 0)
        
        pcall(fti, ball, _rootPart, 0)
        pcall(fti, ball, _rootPart, 1)
        if _hasRightLeg then pcall(fti, ball, _rightLeg, 0); pcall(fti, ball, _rightLeg, 1) end
        if _hasLeftLeg then pcall(fti, ball, _leftLeg, 0); pcall(fti, ball, _leftLeg, 1) end
        local lt = _chainLastT[ball] or 0
        if now - lt < CHAIN_NORM then _chainCount[ball] = (cc + 1) else _chainCount[ball] = 1 end
        _chainLastT[ball] = now
        local totalForce = driftOut + spinPull
        if totalForce > 0.18 then
            local c = math.min(totalForce * 0.80 * stickyK, STICKY_F)
            pcall(function()
                local cur = ball.AssemblyLinearVelocity
                local newX = cur.X - nx*c
                local newZ = cur.Z - nz*c
                local latDrift = (-newX*lz + newZ*lx)
                local latK = clamp(math.abs(latDrift) * 0.05, 0, 0.3)
                newX = newX + lz*latDrift*latK
                newZ = newZ - lx*latDrift*latK
                ball.AssemblyLinearVelocity = V3(newX, cur.Y, newZ)
            end)
        end
        return true
    else
        if dist > GROUND_REACH * 2.5 then _chainCount[ball] = 0 end
    end
    return false
end

-- Dribble input handling
local _mouseHeld = false
local _lastReleaseT = -999
local _lastPressT = -999
local dribbleLast = 0
local _isClicking = false

UIS.InputBegan:Connect(function(inp, gp)
    if gp then return end
    if inp.UserInputType == Enum.UserInputType.MouseButton1 then 
        _mouseHeld = true
        _isClicking = true
        _lastPressT = clock()
    end
end)

UIS.InputEnded:Connect(function(inp, gp)
    if gp then return end
    if inp.UserInputType == Enum.UserInputType.MouseButton1 then 
        _mouseHeld = false
        _isClicking = false
        _lastReleaseT = clock()
    end
end)

-- State flags
local headerActive = false
local headerTouching = false
local airLegActive = false
local airLegTouching = false
local cachedPing = 0.05
local lastPingTime = 0
local lastInsideTime = 0
local _AX = {0,0,0,0,0,0,0,0}
local _AY = {0,0,0,0,0,0,0,0}
local _AZ = {0,0,0,0,0,0,0,0}
local _LAT = {0.20,0.32,0.44,0.56,0.68,0.80,0.94,1.10}
local _avFrame = 0

local _ball = nil
local _ballSearchT = 0

local function getBall()
    if _ball and _ball.Parent then return _ball end
    local now = clock()
    if now - _ballSearchT < 0.05 then return _ball end
    _ballSearchT = now
    if _ball then cleanBall(_ball); _ball = nil end
    local sys = workspace:FindFirstChild("TPSSystem")
    _ball = (sys and sys:FindFirstChild("TPS")) or workspace:FindFirstChild("TPSClientBall_Proxy") or workspace:FindFirstChild("Ball")
    if _ball then addBall(_ball) end
    return _ball
end

-- Ping buffer from first script
local _pBuf = {0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05}
local _pSum = 0.5
local _pIdx = 1

local function updatePing()
    local raw = clamp(LocalPlayer:GetNetworkPing(), 0.008, 0.30)
    _pSum = _pSum - _pBuf[_pIdx] + raw
    _pBuf[_pIdx] = raw
    _pIdx = (_pIdx % 10) + 1
    cachedPing = _pSum * 0.1
end

-- Store connections for refresh
local heartbeatConn = nil
local preSimConn = nil
local postSimConn = nil
local renderConn = nil
local steppedConn = nil

-- Main heartbeat function
local function heartbeatFunc()
    local now = clock()
    local ball = getBall()
    if now - lastPingTime >= 1.8 then 
        lastPingTime = now
        updatePing()
    end
    
    _avFrame = _avFrame + 1
    if _avFrame >= 4 then
        _avFrame = 0
        if ball and _rootPart then
            local rp = _rootPart.Position
            local bp = ball.Position
            local dx = bp.X - rp.X
            local dy = bp.Y - rp.Y
            local dz = bp.Z - rp.Z
            if dx*dx + dy*dy + dz*dz < 576 then
                pcall(function() 
                    local av = ball.AssemblyAngularVelocity
                    if av.X*av.X + av.Y*av.Y + av.Z*av.Z > 0.0001 then 
                        ball.AssemblyAngularVelocity = V3zero 
                    end
                end)
            end
        end
    end
    
    if headerActive and ball and _rootPart then 
        local hp = _head or _rootPart
        pcall(fti, ball, hp, 0)
        pcall(fti, ball, hp, 1)
    end
    if airLegActive and ball and _rootPart then 
        fireLegTouch(ball)
    end
    
    local sinceRelease = now - _lastReleaseT
    local sincePress = now - _lastPressT
    local effectiveDribble = _mouseHeld or (sinceRelease <= CHAIN_FAST) or (sinceRelease <= CHAIN_NORM and sincePress <= CHAIN_NORM * 1.5)
    
    if not (effectiveDribble and ball and _rootPart) then return end
    if _insideNow[ball] then return end
    
    local dynCD = STEP * math.max(1, math.floor(1 + cachedPing * 24))
    if now - dribbleLast < dynCD then return end
    dribbleLast = now
    
    local bp = ball.Position
    local bv = ball.AssemblyLinearVelocity
    local rp = _rootPart.Position
    local bx, by, bz = bp.X, bp.Y, bp.Z
    local vx, vy, vz = bv.X, bv.Y, bv.Z
    local rx, ry, rz = rp.X, rp.Y, rp.Z
    local bspdH = sqrt(vx*vx + vz*vz)
    local spinY = 0
    pcall(function() spinY = ball.AssemblyAngularVelocity.Y end)
    local look = Camera.CFrame.LookVector
    local pax, pay, paz = arcXYZ(bx, by, bz, vx, vy, vz, cachedPing)
    local pdx = pax - rx
    local pdy = pay - ry
    local pdz = paz - rz
    local cc = _chainCount[ball] or 0
    local chainBonus = clamp(cc - CHAIN_BOOST_AT + 1, 0, 3) * CHAIN_BOOST_R
    local reachR = (GROUND_REACH + cachedPing*bspdH*0.95 + clamp(bspdH*0.026, 0, 1)*0.6 + chainBonus) * 1.3
    if pdx*pdx + pdy*pdy + pdz*pdz <= reachR*reachR then
        fireDribble(ball, bx, bz, vx, vz, rx, rz, look.X, look.Z, cachedPing, vy, bspdH, spinY, now)
        if _humanoid and _humanoid.FloorMaterial == Enum.Material.Air and _head then
            local hpos = _head.Position
            local hdx = pax - hpos.X
            local hdy = pay - hpos.Y
            local hdz = paz - hpos.Z
            local effHR = headerR(vx, vy, vz, 0, 0, 0, by - hpos.Y) * 1.9
            if hdx*hdx + hdy*hdy + hdz*hdz <= effHR*effHR then 
                pcall(fti, ball, _head, 0)
                pcall(fti, ball, _head, 1)
            end
        end
    elseif (bx-rx)^2 + (by-ry)^2 + (bz-rz)^2 > (GROUND_REACH*3.2)^2 then
        if not _mouseHeld and sinceRelease > CHAIN_NORM then _lastReleaseT = -999 end
    end
end

local function steppedFunc()
    local ball = getBall()
    if not _rootPart or not ball then return end
    if headerActive then
        local hp = _head or _rootPart
        pcall(fti, ball, hp, 0); pcall(fti, ball, hp, 1)
    end
    if airLegActive then fireLegTouch(ball) end
end

-- PreSimulation function
local function preSimFunc()
    if not _rootPart or not _humanoid then return end
    local ball = getBall()
    if not ball then return end
    
    -- CONSTANT WELD NUKE - ensures no opponent welds survive
    ultraNukeWelds(ball, 0)
    
    local ballPos = ball.Position
    local rootPos = _rootPart.Position
    local dx = ballPos.X - rootPos.X
    local dz = ballPos.Z - rootPos.Z
    if math.abs(dx) > 14 or math.abs(dz) > 14 then return end
    
    local isJumping = _humanoid.FloorMaterial == Enum.Material.Air
    recordBall(ball)
    
    local dy = ballPos.Y - rootPos.Y
    local rawDist = sqrt(dx*dx + dy*dy + dz*dz)
    local ahead = cachedPing + INV60
    
    local bp = ball.Position
    local bv = ball.AssemblyLinearVelocity
    local bx, by, bz = bp.X, bp.Y, bp.Z
    local vx, vy, vz = bv.X, bv.Y, bv.Z
    local rx, ry, rz = rootPos.X, rootPos.Y, rootPos.Z
    local rv = _rootPart.AssemblyLinearVelocity
    local rvx, rvy, rvz = rv.X, rv.Y, rv.Z
    local ddx = bx - rx
    local ddy = by - ry
    local ddz = bz - rz
    local rawDistSq = ddx*ddx + ddy*ddy + ddz*ddz
    
    if rawDistSq > 625 then 
        _insideNow[ball] = false
        return 
    end
    
    local inside = rawDistSq < INSIDE_RSQ
    if not inside then
        for i = 1, 4 do
            local ex = ahead * _LAT[i]
            local px, py, pz = arcXYZ(bx, by, bz, vx, vy, vz, ex)
            local exd = px - rx
            local eyd = py - ry
            local ezd = pz - rz
            if exd*exd + eyd*eyd + ezd*ezd < INSIDE_RSQ then
                inside = true
                break
            end
        end
    end
    _insideNow[ball] = inside
    
    if inside then
        local last = _insideLast[ball] or 0
        if clock() - last > INSIDE_CD then
            _insideLast[ball] = clock()
            fireInsideBurst(ball)
            nukeWeldsDeep(ball)
        end
        return
    end
    
    -- Inside burst check
    if rawDist < INSIDE_THRESH then
        local now = clock()
        if now - lastInsideTime >= INSIDE_CD then
            lastInsideTime = now
            fireInsideBurst(ball)
        end
    end
    
    local predPos, predVel
    if cachedPing >= HIGH_PING_MS or rawDist >= 7 then
        predPos, predVel = smartPredict(ball, ahead + INV60)
    else
        predPos = ballPos
        predVel = ball.AssemblyLinearVelocity
    end
    
    local pvx, pvy, pvz = predVel.X, predVel.Y, predVel.Z
    local fpx = predPos.X + pvx*INV60
    local fpy = predPos.Y + pvy*INV60 + GRAV_HALF_SQ
    local fpz = predPos.Z + pvz*INV60
    
    local dx_pred = predPos.X - rootPos.X
    local dy_pred = predPos.Y - rootPos.Y
    local dz_pred = predPos.Z - rootPos.Z
    local fdx = fpx - rootPos.X
    local fdy = fpy - rootPos.Y
    local fdz = fpz - rootPos.Z
    
    -- Prediction array for first script
    local s = ahead + INV60
    _AX[1],_AY[1],_AZ[1] = arcXYZ(bx,by,bz,vx,vy,vz,s*0.16)
    _AX[2],_AY[2],_AZ[2] = arcXYZ(bx,by,bz,vx,vy,vz,s*0.30)
    _AX[3],_AY[3],_AZ[3] = arcXYZ(bx,by,bz,vx,vy,vz,s*0.44)
    _AX[4],_AY[4],_AZ[4] = arcXYZ(bx,by,bz,vx,vy,vz,s*0.58)
    _AX[5],_AY[5],_AZ[5] = arcXYZ(bx,by,bz,vx,vy,vz,s*0.72)
    _AX[6],_AY[6],_AZ[6] = arcXYZ(bx,by,bz,vx,vy,vz,s*0.86)
    _AX[7],_AY[7],_AZ[7] = arcXYZ(bx,by,bz,vx,vy,vz,s*1.00)
    _AX[8],_AY[8],_AZ[8] = arcXYZ(bx,by,bz,vx,vy,vz,s*1.14)
    
    if isJumping then
        local dynX, dynZ, dynY = calcDynamicReach(ballPos.Y, pvx, pvy, pvz, rootPos.Y)
        
        local inHeader = (dy_pred > 0.8 and math.abs(dx_pred) <= dynX and math.abs(dz_pred) <= dynZ and dy_pred <= dynY)
                      or (fdy > 0.8 and math.abs(fdx) <= dynX and math.abs(fdz) <= dynZ and fdy <= dynY)
        
        if inHeader then
            local headPart = _head or _rootPart
            pcall(fti, ball, headPart, 0)
            pcall(fti, ball, headPart, 1)
            
            local headPos = _head and _head.Position or rootPos
            local phx_base = headPos.X + rvx*ahead
            local phy_base = headPos.Y + rvy*ahead
            local phz_base = headPos.Z + rvz*ahead
            
            local phx = predPos.X - phx_base
            local phy = predPos.Y - phy_base
            local phz = predPos.Z - phz_base
            local predHDSq = phx*phx + phy*phy + phz*phz
            
            local fhx = fpx - phx_base
            local fhy = fpy - phy_base
            local fhz = fpz - phz_base
            local farHDSq = fhx*fhx + fhy*fhy + fhz*fhz
            
            local toHX = phx_base - predPos.X
            local toHY = phy_base - predPos.Y
            local toHZ = phz_base - predPos.Z
            local toHL = sqrt(toHX*toHX + toHY*toHY + toHZ*toHZ)
            local convergence = 0
            if toHL > 0.001 then
                local iLen = 1 / toHL
                convergence = pvx*(toHX*iLen) + pvy*(toHY*iLen) + pvz*(toHZ*iLen)
            end
            local convBonus = clamp(convergence * (1/40), 0, 1) * 0.9
            local effR = (1.6 + convBonus) * 0.75
            local effContactSq = effR * effR
            
            if predHDSq <= effContactSq or farHDSq <= effContactSq then
                if not headerTouching then
                    headerTouching = true
                    triggerContactFlag(1)
                    -- NUKE WELDS on header contact
                    ultraNukeWelds(ball, 0)
                    pcall(function()
                        local cur = ball.AssemblyLinearVelocity
                        if cur.Magnitude < 80 then
                            local look = Camera.CFrame.LookVector
                            local fallSpd = math.max(-cur.Y, 0)
                            local boost = clamp(fallSpd * (1/40), 0, 1)
                            local power = HEADER_POWER * (1 + boost * 2.5)
                            ball.AssemblyLinearVelocity = V3(look.X*power*60, cur.Y, look.Z*power*60)
                        end
                    end)
                    nukeWeldsDeep(ball)
                end
                if not headerActive then headerActive = true end
            else
                headerTouching = false
                headerActive = false
            end
        else
            headerActive = false
            headerTouching = false
        end
        
        local inVolley = false
        for i = 1, 8 do
            local ex = _AX[i] - rx
            local ey = _AY[i] - ry
            local ez = _AZ[i] - rz
            if ex*ex + ez*ez <= 3.5*3.5 and ey <= 3.5 and ey > -3 then
                inVolley = true
                break
            end
        end
        
        if inVolley then
            fireLegTouch(ball)
            if rawDist <= 3.5 then
                if not airLegTouching then
                    airLegTouching = true
                    triggerContactFlag(2)
                    -- NUKE WELDS on volley/air leg contact
                    ultraNukeWelds(ball, 0)
                    pcall(function() 
                        local cur = ball.AssemblyLinearVelocity
                        if cur.Y > -50 then 
                            ball.AssemblyLinearVelocity = V3(cur.X, -math.abs(cur.Y) - 48, cur.Z)
                        end
                    end)
                end
                if not airLegActive then airLegActive = true end
            else
                if airLegTouching then
                    airLegTouching = false
                    airLegActive = false
                end
            end
        else
            if airLegActive then
                airLegActive = false
                airLegTouching = false
            end
        end
    else
        -- Ground state
        if headerActive then headerActive = false end
        if airLegActive then airLegActive = false end
        headerTouching = false
        airLegTouching = false
        
        local fired = false
        for i = 1, 8 do
            if not fired then
                local ex = _AX[i] - rx
                local ey = _AY[i] - ry
                local ez = _AZ[i] - rz
                if math.abs(ex) <= GROUND_REACH and math.abs(ez) <= GROUND_REACH and math.abs(ey) <= 2.8 then 
                    fireLegTouch(ball)
                    fired = true
                end
            end
        end
        
        if not fired then
            local bspdH = sqrt(vx*vx + vz*vz)
            local extR = GROUND_REACH + cachedPing*bspdH*0.7
            local extRsq = extR * extR
            for i = 1, 8 do
                if not fired then
                    local ex = _AX[i] - rx
                    local ey = _AY[i] - ry
                    local ez = _AZ[i] - rz
                    if ex*ex + ey*ey + ez*ez <= extRsq then 
                        fireLegTouch(ball)
                        fired = true
                    end
                end
            end
        end
        
        if not fired and rawDistSq <= (GROUND_REACH*1.8)^2 then 
            fireLegTouch(ball)
        end
    end
end

-- PostSimulation function
local function postSimFunc()
    local ball = getBall()
    if not _rootPart or not ball then return end
    if headerActive then
        local hp = _head or _rootPart
        pcall(fti, ball, hp, 0)
        pcall(fti, ball, hp, 1)
    end
    if airLegActive then fireLegTouch(ball) end
end

-- RenderStepped function for TPS Ball Magnet
local function renderFunc()
    local ConfigLocal = {
        MagnetStrength = 2,
        BallOffset = Vector3.new(0, -2.8, 2.2)
    }
    local BallTPS = Workspace:FindFirstChild("BallTPS")
    local ClientBallProxy_TPS = Workspace:FindFirstChild("ClientBallProxy_TPS")
    if BallTPS and ClientBallProxy_TPS and _rootPart then
        local targetCFrame = _rootPart.CFrame * CFrame.new(ConfigLocal.BallOffset)
        BallTPS.CFrame = targetCFrame
        ClientBallProxy_TPS.CFrame = targetCFrame
        BallTPS.CanCollide = false
        BallTPS.AssemblyLinearVelocity = Vector3.zero
        BallTPS.AssemblyAngularVelocity = Vector3.zero
        ClientBallProxy_TPS.CanCollide = false
        ClientBallProxy_TPS.AssemblyLinearVelocity = Vector3.zero
        ClientBallProxy_TPS.AssemblyAngularVelocity = Vector3.zero
    end
end

-- Function to refresh all connections
local function refreshAllConnections()
    if heartbeatConn then heartbeatConn:Disconnect() end
    if preSimConn then preSimConn:Disconnect() end
    if postSimConn then postSimConn:Disconnect() end
    if renderConn then renderConn:Disconnect() end
    if steppedConn then steppedConn:Disconnect() end
    
    heartbeatConn = RunService.Heartbeat:Connect(heartbeatFunc)
    preSimConn = RunService.PreSimulation:Connect(preSimFunc)
    postSimConn = RunService.PostSimulation:Connect(postSimFunc)
    renderConn = RunService.RenderStepped:Connect(renderFunc)
    steppedConn = RunService.Stepped:Connect(steppedFunc)
end

-- Start initial connections
refreshAllConnections()

-- ===== PERSISTENT FLAG MANAGER - OPTIMIZED (NO LAG) =====
local aerialModeActive = false
local lastAerialCheck = false

-- Apply flags only when state actually changes (not every frame)
task.spawn(function()
    while true do
        task.wait(0.1) -- Check every 100ms (was 50ms, now half as frequent)
        if not _humanoid then continue end
        local shouldBeAerial = (_humanoid.FloorMaterial == Enum.Material.Air) or headerActive or airLegActive
        
        -- Only apply if state actually changed
        if shouldBeAerial and not lastAerialCheck then
            applyFlagConfigOptimized(AERIAL_FLAGS)
            aerialModeActive = true
        elseif not shouldBeAerial and lastAerialCheck then
            applyFlagConfigOptimized(NORMAL_FLAGS)
            aerialModeActive = false
        end
        lastAerialCheck = shouldBeAerial
    end
end)

-- INFINITY LOOP: Refresh connections every 60 seconds
task.spawn(function()
    while true do
        task.wait(60)
        -- Reset all state flags
        headerActive = false
        headerTouching = false
        airLegActive = false
        airLegTouching = false
        _mouseHeld = false
        _isClicking = false
        _avFrame = 0
        dribbleLast = 0
        
        -- Refresh connections
        refreshAllConnections()
        
        -- Refresh character
        refreshCharacter(LocalPlayer.Character)
        
        -- Retrack balls
        for ball in pairs(_allBalls) do
            cleanBall(ball)
        end
        _allBalls = {}
        _ballConns = {}
        trackAllBalls()
    end
end)

-- Cleanup on death
LocalPlayer.CharacterRemoving:Connect(function()
    _mouseHeld = false
    _isClicking = false
    _lastReleaseT = -999
    _lastPressT = -999
    headerActive = false
    headerTouching = false
    airLegActive = false
    airLegTouching = false
    flagActive[1] = false
    flagActive[2] = false
    for i = 1, 2 do
        if flagRestoreThreads[i] then
            pcall(task.cancel, flagRestoreThreads[i])
            flagRestoreThreads[i] = nil
        end
    end
    applyFlagConfigOptimized(NORMAL_FLAGS)
    aerialModeActive = false
    lastAerialCheck = false
end)

pcall(function() settings().Physics.AllowSleep = false end)

-- Moss Kick feature
local function fireKick()
    -- Placeholder for kick function
end

do
    local headSize = 2
    
    local function isBoxTouching(playerCFrame, playerSize, targetCFrame, targetSize)
        local playerMin = playerCFrame.Position - playerSize/2
        local playerMax = playerCFrame.Position + playerSize/2
        local targetMin = targetCFrame.Position - targetSize/2
        local targetMax = targetCFrame.Position + targetSize/2
        return (playerMin.X <= targetMax.X and playerMax.X >= targetMin.X)
           and (playerMin.Y <= targetMax.Y and playerMax.Y >= targetMin.Y)
           and (playerMin.Z <= targetMax.Z and playerMax.Z >= targetMin.Z)
    end
    
    local mossWasInside = false
    local mossCooldown = false
    local mossConnectionLocal = nil
    
    local function startMoss()
        if mossConnectionLocal then mossConnectionLocal:Disconnect() end
        local character = LocalPlayer.Character
        if not character then return end
        local head = character:WaitForChild("Head")
        local hrp = character:WaitForChild("HumanoidRootPart")
        mossConnectionLocal = RunService.PreSimulation:Connect(function()
            local char = LocalPlayer.Character
            if not char then return end
            head = char:FindFirstChild("Head")
            hrp = char:FindFirstChild("HumanoidRootPart")
            if not head or not hrp then return end
            
            local targets = {}
            local BallsFolder = Workspace:FindFirstChild("Balls")
            if BallsFolder then
                for _, ball in BallsFolder:GetChildren() do
                    if ball:IsA("BasePart") then table.insert(targets, ball) end
                end
            end
            local TPSSystem = Workspace:FindFirstChild("TPSSystem")
            if TPSSystem then
                for _, tpsPart in TPSSystem:GetChildren() do
                    if tpsPart:IsA("BasePart") or tpsPart.Name == "TPS" then
                        table.insert(targets, tpsPart)
                    end
                end
            end
            
            local predTime = 1e-13
            local ballCurrentlyInside = false
            for _, target in targets do
                local predictedPos = target.Position + target.Velocity * predTime
                local predictedCFrame = CFrame.new(predictedPos) * target.CFrame.Rotation
                if target.Position.Y >= hrp.Position.Y then
                    if isBoxTouching(head.CFrame, Vector3.new(headSize, headSize, headSize), predictedCFrame, target.Size) then
                        ballCurrentlyInside = true
                        break
                    end
                end
            end
            
            if ballCurrentlyInside and not mossWasInside and not mossCooldown then
                mossCooldown = true
                fireKick()
                task.delay(0.5, function()
                    mossCooldown = false
                end)
            end
            mossWasInside = ballCurrentlyInside
        end)
    end
    
    LocalPlayer.CharacterAdded:Connect(function()
        mossWasInside = false
        mossCooldown = false
        startMoss()
    end)
    
    if LocalPlayer.Character then
        startMoss()
    end
end

-- CONTINUOUS WELD CLEANUP THREAD - Optimized (0.05s instead of 0.016s)
task.spawn(function()
    while true do
        task.wait(0.05) -- Every 50ms (was 16ms, now less CPU)
        local ball = getBall()
        if ball then
            ultraNukeWelds(ball, 0)
        end
    end
end)

task.spawn(function()
    while true do
        task.wait(60)
        for ball in pairs(_allBalls) do
            if not ball.Parent then cleanBall(ball) end
        end
    end
end)
