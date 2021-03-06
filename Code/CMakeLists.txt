PROJECT(Code)
INCLUDE_REGULAR_EXPRESSION("^.*$")

#---- Directories -----------------------------------------------------------------------

SET(TOOLS_LIBRARY_SOURCE_DIR             ${Code_SOURCE_DIR}/Tools)
SET(MATHS_LIBRARY_SOURCE_DIR             ${Code_SOURCE_DIR}/Maths)
SET(DENOISING_LIBRARY_SOURCE_DIR         ${Code_SOURCE_DIR}/Denoising)
SET(DIFFUSION_LIBRARY_SOURCE_DIR         ${Code_SOURCE_DIR}/Diffusion)
SET(TRACTOGRAPHY_LIBRARY_SOURCE_DIR      ${Code_SOURCE_DIR}/Tractography)
set(FEATURE_SELECTION_LIBRARY_SOURCE_DIR ${Code_SOURCE_DIR}/FeatureSelection)

IF(BUILD_TESTING)
    SET(MATHS_TESTS_SOURCE_DIR ${MATHS_LIBRARY_SOURCE_DIR}/Tests)
ENDIF(BUILD_TESTING)

INCLUDE_DIRECTORIES(
    ${MATHS_LIBRARY_SOURCE_DIR}
    ${TOOLS_LIBRARY_SOURCE_DIR}
    ${DIFFUSION_LIBRARY_SOURCE_DIR}
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}
    ${DENOISING_LIBRARY_SOURCE_DIR}
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}
    ${Code_SOURCE_DIR}/Transformations
    ${Code_SOURCE_DIR}/Reconstruction
)

#---- Tools library ---------------------------------------------------------------------

SET(TOOLS_LIBRARY_HEADER
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkMacro.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkFileHelper.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkNrrdField.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkLandmarksFileReader.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkIOTransformHelper.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkImageHelper.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceFileHelper.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceHelper.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkCommandIterationUpdate.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkCommandProgressUpdate.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkIOImageHelper.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkPatch.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkPatchTool.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkNoise.h
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkRegionGrow.h
)

SET(TOOLS_LIBRARY_SOURCES
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkFileHelper.cxx
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkNrrdField.cxx
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkLandmarksFileReader.cxx
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceFileHelper.cxx
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceHelper.cxx
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkIOImageHelper.cxx   
    ${TOOLS_LIBRARY_SOURCE_DIR}/btkRegionGrow.cxx
)

ADD_LIBRARY(btkToolsLibrary STATIC ${TOOLS_LIBRARY_HEADER} ${TOOLS_LIBRARY_SOURCES})
TARGET_LINK_LIBRARIES(btkToolsLibrary ${ITK_LIBRARIES})

#---- Maths library ---------------------------------------------------------------------

SET(MATHS_LIBRARY_HEADER
    ${MATHS_LIBRARY_SOURCE_DIR}/btkMathFunctions.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkSphericalDirection.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkLegendrePolynomial.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkSphericalHarmonics.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkMatrixOperations.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkHistogram.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkJointHistogram.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkProbabilityDensity.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkNormalProbabilityDensity.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkVonMisesFisherProbabilityDensity.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkCurvatures.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkPSF.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkBoxCarPSF.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkSincPSF.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkGaussianPSF.h
    ${MATHS_LIBRARY_SOURCE_DIR}/btkHybridPSF.h


)

SET(MATHS_LIBRARY_SOURCES
    ${MATHS_LIBRARY_SOURCE_DIR}/btkMathFunctions.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkSphericalDirection.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkLegendrePolynomial.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkSphericalHarmonics.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkMatrixOperations.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkHistogram.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkJointHistogram.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkNormalProbabilityDensity.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkVonMisesFisherProbabilityDensity.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkCurvatures.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkPSF.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkBoxCarPSF.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkSincPSF.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkGaussianPSF.cxx
    ${MATHS_LIBRARY_SOURCE_DIR}/btkHybridPSF.cxx
)

ADD_LIBRARY(btkMathsLibrary STATIC ${MATHS_LIBRARY_HEADER} ${MATHS_LIBRARY_SOURCES})

IF(BUILD_TESTING)
    ADD_EXECUTABLE(btkMathsLibraryTestsApp ${MATHS_TESTS_SOURCE_DIR}/btkMathsLibraryTestsApp.cxx
        ${MATHS_TESTS_SOURCE_DIR}/btkMatrixOperationsTest.cxx
        ${MATHS_TESTS_SOURCE_DIR}/btkLegendrePolynomialTest.cxx
        ${MATHS_TESTS_SOURCE_DIR}/btkMathFunctionsTest.cxx
        ${MATHS_TESTS_SOURCE_DIR}/btkSphericalHarmonicsTest.cxx
        ${MATHS_TESTS_SOURCE_DIR}/btkNormalProbabilityDensityTest.cxx
        ${MATHS_TESTS_SOURCE_DIR}/btkVonMisesFisherProbabilityDensityTest.cxx
    )
    TARGET_LINK_LIBRARIES(btkMathsLibraryTestsApp btkDiffusionLibrary btkMathsLibrary ${CPPUNIT_LIBRARY} ${ITK_LIBRARIES})
    ADD_TEST(btkMathsLibraryTests btkMathsLibraryTestsApp)
ENDIF(BUILD_TESTING)

#---- Denoising library ---------------------------------------------------------------------

SET(DENOISING_LIBRARY_HEADER
    ${DENOISING_LIBRARY_SOURCE_DIR}/btkPseudoResidualsVarianceCalculator.h
)

SET(DENOISING_LIBRARY_SOURCES
    ${DENOISING_LIBRARY_SOURCE_DIR}/btkPseudoResidualsVarianceCalculator.cxx
)

ADD_LIBRARY(btkDenoisingLibrary STATIC ${DENOISING_LIBRARY_HEADER} ${DENOISING_LIBRARY_SOURCES})
#TARGET_LINK_LIBRARIES(btkDenoisingLibrary btkToolsLibrary ${ITK_LIBRARIES})

#---- Diffusion library -----------------------------------------------------------------

SET(DIFFUSION_LIBRARY_HEADER
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkGradientDirection.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequence.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceFileReader.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceFileWriter.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionGradientTable.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionTensorReconstructionFilter.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkSphericalHarmonicsDiffusionDecompositionFilter.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionModel.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkTensorModel.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkOrientationDiffusionFunctionModel.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSignal.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceToDiffusionSignalFilter.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSlice.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionDataset.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkWeightedEstimationBase.h
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkWeightedEstimationFilter.h



)

SET(DIFFUSION_LIBRARY_SOURCES
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkGradientDirection.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequence.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceFileReader.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceFileWriter.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionTensorReconstructionFilter.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkSphericalHarmonicsDiffusionDecompositionFilter.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionModel.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkTensorModel.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkOrientationDiffusionFunctionModel.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSignal.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSequenceToDiffusionSignalFilter.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionSlice.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkDiffusionDataset.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkWeightedEstimationBase.cxx
    ${DIFFUSION_LIBRARY_SOURCE_DIR}/btkWeightedEstimationFilter.cxx


)

ADD_LIBRARY(btkDiffusionLibrary STATIC ${DIFFUSION_LIBRARY_HEADER} ${DIFFUSION_LIBRARY_SOURCES})
TARGET_LINK_LIBRARIES(btkDiffusionLibrary btkToolsLibrary btkMathsLibrary)

#---- Tractography library --------------------------------------------------------------

SET(TRACTOGRAPHY_LIBRARY_HEADER
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkTractographyAlgorithm.h
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkStreamlineTractographyAlgorithm.h
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkParticleFilteringTractographyAlgorithm.h
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkPolyDataColorLinesByOrientation.h
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkParticle.h
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkImportanceDensity.h
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkLikelihoodDensity.h
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkPriorDensity.h
)

SET(TRACTOGRAPHY_LIBRARY_SOURCES
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkTractographyAlgorithm.cxx
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkStreamlineTractographyAlgorithm.cxx
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkParticleFilteringTractographyAlgorithm.cxx
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkPolyDataColorLinesByOrientation.cxx
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkParticle.cxx
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkImportanceDensity.cxx
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkLikelihoodDensity.cxx
    ${TRACTOGRAPHY_LIBRARY_SOURCE_DIR}/btkPriorDensity.cxx
)

ADD_LIBRARY(btkTractographyLibrary STATIC ${TRACTOGRAPHY_LIBRARY_HEADER} ${TRACTOGRAPHY_LIBRARY_SOURCES})
TARGET_LINK_LIBRARIES(btkTractographyLibrary ${ITK_LIBRARIES})


#---- Feature selection library ---------------------------------------------------------

set(FEATURE_SELECTION_LIBRARY_HEADER
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkFeatureSelectionCostFunction.h
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkNadarayaWatsonReconstructionErrorFunction.h
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkFeatureSelectionAlgorithm.h
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkGreedyFeatureSelectionAlgorithm.h
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkFeatureSelectionCommandIterationUpdate.h
)

set(FEATURE_SELECTION_LIBRARY_SOURCES
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkFeatureSelectionCostFunction.cxx
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkNadarayaWatsonReconstructionErrorFunction.cxx
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkFeatureSelectionAlgorithm.cxx
    ${FEATURE_SELECTION_LIBRARY_SOURCE_DIR}/btkGreedyFeatureSelectionAlgorithm.cxx
)

add_library(btkFeatureSelectionLibrary STATIC ${FEATURE_SELECTION_LIBRARY_HEADER} ${FEATURE_SELECTION_LIBRARY_SOURCES})
