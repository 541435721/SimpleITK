
set(ITK_TEST_DRIVER  "itkTestDriver")

# Test data directory
set(TEST_HARNESS_TEMP_DIRECTORY ${SimpleITK_BINARY_DIR}/Testing/Temporary)
set(TEST_HARNESS_DATA_DIRECTORY ${SimpleITK_BINARY_DIR}/ExternalData/Testing/Data)

#
# CXX Examples
#
sitk_add_test(NAME CXX.Example.SimpleGaussian
  COMMAND "${ITK_TEST_DRIVER}"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXX.SimpleGaussian.nrrd"
      5ef06ed0d5d8952c770348699b70f23e
    $<TARGET_FILE:SimpleGaussian>
     DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
     2.0
     "${TEST_HARNESS_TEMP_DIRECTORY}/CXX.SimpleGaussian.nrrd"
  )

sitk_add_test(NAME CXX.Example.SimpleGaussianFunctional
  COMMAND "${ITK_TEST_DRIVER}"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXX.SimpleGaussianFunctional.nrrd"
      5ef06ed0d5d8952c770348699b70f23e
    $<TARGET_FILE:SimpleGaussianFunctional>
     DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
     2.0
     "${TEST_HARNESS_TEMP_DIRECTORY}/CXX.SimpleGaussianFunctional.nrrd"
  )

sitk_add_test(NAME CXX.Example.FilterProgressReporting
  COMMAND "${ITK_TEST_DRIVER}"
    $<TARGET_FILE:FilterProgressReporting>
    DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
    2.0
    "${TEST_HARNESS_TEMP_DIRECTORY}/CXX.FilterProgressReporting.nrrd"
  )
set_tests_properties( CXX.Example.FilterProgressReporting
  PROPERTIES PASS_REGULAR_EXPRESSION "DiscreteGaussianImageFilter Progress" )

sitk_add_test(NAME CXX.Example.DicomSeriesReader
  COMMAND "${ITK_TEST_DRIVER}"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXX.DicomSeriesReader.nrrd"
      "8e65f711d450b6b466c6d76a667ecb72"
    $<TARGET_FILE:DicomSeriesReader>
      DATA{${SimpleITK_DATA_ROOT}/Input/DicomSeries/,REGEX:Image[0-9]+.dcm}
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXX.DicomSeriesReader.nrrd"
  )

sitk_add_test(NAME  CXX.Example.Segmentation.ConnectedThresholdImageFilter1
  COMMAND "${ITK_TEST_DRIVER}"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXXSegmentationConnectedThreshold1.png"
      "d4ff9c512f9c18a84729677de800db0d"
    $<TARGET_FILE:ConnectedThresholdImageFilter>
      DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySlice.png}
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXXSegmentationConnectedThreshold1.png"
      150 180
      60 116
  )

sitk_add_test(NAME  CXX.Example.Segmentation.ConnectedThresholdImageFilter2
  COMMAND "${ITK_TEST_DRIVER}"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXXSegmentationConnectedThreshold2.png"
      "a9695df04cde5b9c5481d8c253a5b3aa"
    $<TARGET_FILE:ConnectedThresholdImageFilter>
      DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySlice.png}
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXXSegmentationConnectedThreshold2.png"
      210 250
      81 112
  )

sitk_add_test( NAME CXX.Example.Segmentation.NeighborhoodConnectedImageFilter1
  COMMAND "${ITK_TEST_DRIVER}"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXXSegmentationNeighborhoodConnected1.png"
      "311d9d4b492e1ea625e3b0b295814dce"
    $<TARGET_FILE:NeighborhoodConnectedImageFilter>
      DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySlice.png}
      "${TEST_HARNESS_TEMP_DIRECTORY}/CXXSegmentationNeighborhoodConnected1.png"
      150 180
      60 116
  )

sitk_add_test(NAME CXX.Example.ImageRegistrationMethod1
  COMMAND
  $<TARGET_FILE:ImageRegistrationMethod1>
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/CXXImageRegistrationMethod1Test.hdf5
  )

sitk_add_test(NAME CXX.Example.ImageRegistrationMethod2
  COMMAND
    $<TARGET_FILE:ImageRegistrationMethod2>
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/CXXImageRegistrationMethod2Test.hdf5
  )

sitk_add_test(NAME CXX.Example.DemonsRegistration1
  COMMAND
    $<TARGET_FILE:DemonsRegistration1>
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBSplined10.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/CXXDemonsRegistration1Test.hdf5
  )

sitk_add_test(NAME CXX.Example.DemonsRegistration2
  COMMAND
   $<TARGET_FILE:DemonsRegistration2>
   DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
   DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBSplined10.png}
   DATA{${SimpleITK_DATA_ROOT}/Input/xforms/translation_-13x-17y.txt}
   ${TEST_HARNESS_TEMP_DIRECTORY}/CXXDemonsRegistration2Test.hdf5
  )



#
# Python Examples
#
if ( WRAP_PYTHON )
  sitk_add_python_test( Example.SimpleGaussian
    "${SimpleITKExamples_SOURCE_DIR}/SimpleGaussian.py"
    --compare
      "${TEST_HARNESS_TEMP_DIRECTORY}/Python.SimpleGaussian.nrrd"
      DATA{${SimpleITK_DATA_ROOT}/Baseline/Example_SimpleGaussian_2.0.nrrd}
    DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
    "2.0"
    "${TEST_HARNESS_TEMP_DIRECTORY}/Python.SimpleGaussian.nrrd" )

  sitk_add_python_test( Example.SimpleGaussianProcedural
    "${SimpleITKExamples_SOURCE_DIR}/SimpleGaussianProcedural.py"
    --compare
      "${TEST_HARNESS_TEMP_DIRECTORY}/Python.SimpleGaussianProcedural.nrrd"
      DATA{${SimpleITK_DATA_ROOT}/Baseline/Example_SimpleGaussian_2.0.nrrd}
    DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
    "2.0"
    "${TEST_HARNESS_TEMP_DIRECTORY}/Python.SimpleGaussianProcedural.nrrd" )

  sitk_add_python_test( Example.ImageCreateAndSet
    "${SimpleITKExamples_SOURCE_DIR}/ImageCreateAndSet.py" )

  sitk_add_python_test( Example.CannyEdge
    "${SimpleITKExamples_SOURCE_DIR}/CannyEdge.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/OAS1_0001_MR1_mpr-1_anon.nrrd} )

  sitk_add_python_test( Example.ExtractSlice
    "${SimpleITKExamples_SOURCE_DIR}/ExtractSlice.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/OAS1_0001_MR1_mpr-1_anon.nrrd}
    "30"
    "${TEST_HARNESS_TEMP_DIRECTORY}/Python.ExtractSlice.nrrd")

  sitk_add_python_test( Example.BoarderSegmentation
    "${SimpleITKExamples_SOURCE_DIR}/BoarderSegmentation.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/OAS1_0001_MR1_mpr-1_anon.nrrd}
    "${TEST_HARNESS_TEMP_DIRECTORY}/Python.BoarderSegmentation.nrrd"
    "5000"
    "150" )

 sitk_add_python_test( Example.N4BiasFieldCorrection
    "${SimpleITKExamples_SOURCE_DIR}/N4BiasFieldCorrection.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/OAS1_0001_MR1_mpr-1_anon.nrrd}
    "${TEST_HARNESS_TEMP_DIRECTORY}/Python.Example.N4BiasFieldCorrection.nrrd"
    "1" )

  sitk_add_python_test( Example.ReadAndShow
    "${SimpleITKExamples_SOURCE_DIR}/ReadAndShow.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/VM1111Shrink-RGB.png} )

  sitk_add_python_test( Example.GeodesicActiceContourSegmentation
    "${SimpleITKExamples_SOURCE_DIR}/GeodesicActiceContourSegmentation.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/cthead1.png}
    "${TEST_HARNESS_TEMP_DIRECTORY}/Python.Example.GeodesicActiceContourSegmentation.nrrd"
    .3 10 .9 50 55
    )

  sitk_add_python_test( Example.FFTConvolution
    "${SimpleITKExamples_SOURCE_DIR}/FFTConvolution.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/cthead1-Float.mha}
    DATA{${SimpleITK_DATA_ROOT}/Input/Gaussian_1.5.nrrd}
    "${TEST_HARNESS_TEMP_DIRECTORY}/Python.Examples.FFTConvolution.nrrd"
    )

  sitk_add_python_test( Example.FilterProgressReporting
    "${SimpleITKExamples_SOURCE_DIR}/FilterProgressReporting.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
    "2.0"
    "${TEST_HARNESS_TEMP_DIRECTORY}/PythonFilterProgressReporting.nrrd"
    )

  sitk_add_python_test( Example.DicomSeriesReader
    "${SimpleITKExamples_SOURCE_DIR}/DicomSeriesReader.py"
    --compare-MD5
    "${TEST_HARNESS_TEMP_DIRECTORY}/DicomSeriesReader.nrrd"
    "8e65f711d450b6b466c6d76a667ecb72"
      DATA{${SimpleITK_DATA_ROOT}/Input/DicomSeries/,REGEX:Image[0-9]+.dcm}
      "${TEST_HARNESS_TEMP_DIRECTORY}/DicomSeriesReader.nrrd"
    )

  set_tests_properties( Python.Example.FilterProgressReporting
    PROPERTIES PASS_REGULAR_EXPRESSION "DiscreteGaussianImageFilter Progress" )

  sitk_add_python_test( Example.Segmentation.ConnectedThresholdImageFilter1
    "${SimpleITKExamples_SOURCE_DIR}/Segmentation/ConnectedThresholdImageFilter.py"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/PythonSegmentationConnectedThreshold1.png"
      "d4ff9c512f9c18a84729677de800db0d"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySlice.png}
    "${TEST_HARNESS_TEMP_DIRECTORY}/PythonSegmentationConnectedThreshold1.png"
    150 180
    60 116
    )

  sitk_add_python_test( Example.Segmentation.ConnectedThresholdImageFilter2
    "${SimpleITKExamples_SOURCE_DIR}/Segmentation/ConnectedThresholdImageFilter.py"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/PythonSegmentationConnectedThreshold2.png"
      "a9695df04cde5b9c5481d8c253a5b3aa"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySlice.png}
    "${TEST_HARNESS_TEMP_DIRECTORY}/PythonSegmentationConnectedThreshold2.png"
    210 250
    81 112
    )

  sitk_add_python_test( Example.Segmentation.NeighborhoodConnectedImageFilter1
    "${SimpleITKExamples_SOURCE_DIR}/Segmentation/NeighborhoodConnectedImageFilter.py"
    --compare-MD5
      "${TEST_HARNESS_TEMP_DIRECTORY}/PythonSegmentationNeighborhoodConnected1.png"
      "311d9d4b492e1ea625e3b0b295814dce"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySlice.png}
    "${TEST_HARNESS_TEMP_DIRECTORY}/PythonSegmentationNeighborhoodConnected1.png"
    150 180
    60 116
    )

  sitk_add_python_test( Example.ImageRegistrationMethod1
    "${SimpleITKExamples_SOURCE_DIR}/ImageRegistrationMethod1.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/PythonImageRegistrationMethod11Test.hdf5
    )

  sitk_add_python_test( Example.ImageRegistrationMethod2
    "${SimpleITKExamples_SOURCE_DIR}/ImageRegistrationMethod2.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/PythonImageRegistrationMethod2Test.hdf5
    )

  sitk_add_python_test( Example.ImageRegistrationMethod4Test1
    "${SimpleITKExamples_SOURCE_DIR}/ImageRegistrationMethod4.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/PythonImageRegistrationMethod4Test1.hdf5
    24 1.0
    )

  sitk_add_python_test( Example.ImageRegistrationMethod4Test2
    "${SimpleITKExamples_SOURCE_DIR}/ImageRegistrationMethod4.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainT1SliceBorder20.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/PythonImageRegistrationMethod4Test2.hdf5
    24 0.1
    )

  sitk_add_python_test( Example.ImageRegistrationMethod4Test3
    "${SimpleITKExamples_SOURCE_DIR}/ImageRegistrationMethod4.py"
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20Mask.png}
    DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20Mask.png}
    ${TEST_HARNESS_TEMP_DIRECTORY}/PythonImageRegistrationMethod4Test3.hdf5
    5 0.4
    )

  sitk_add_python_test(Example.DemonsRegistration1
      "${SimpleITKExamples_SOURCE_DIR}/DemonsRegistration1.py"
      DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBorder20.png}
      DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBSplined10.png}
      ${TEST_HARNESS_TEMP_DIRECTORY}/PythonDemonsRegistration1Test.hdf5
    )

  sitk_add_python_test(Example.DemonsRegistration2
      "${SimpleITKExamples_SOURCE_DIR}/DemonsRegistration2.py"
      DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceShifted13x17y.png}
      DATA{${SimpleITK_DATA_ROOT}/Input/BrainProtonDensitySliceBSplined10.png}
      DATA{${SimpleITK_DATA_ROOT}/Input/xforms/translation_-13x-17y.txt}
      ${TEST_HARNESS_TEMP_DIRECTORY}/PythonDemonsRegistration2Test.hdf5
    )

endif ( WRAP_PYTHON )


#
# Lua Examples
#
if ( WRAP_LUA )
  sitk_add_lua_test( Example.SimpleGaussian
    "${SimpleITKExamples_SOURCE_DIR}/SimpleGaussian.lua"
    --compare
      "${TEST_HARNESS_TEMP_DIRECTORY}/Lua.SimpleGaussian.nrrd"
      DATA{${SimpleITK_DATA_ROOT}/Baseline/Example_SimpleGaussian_2.0.nrrd}
    DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
    "2.0"
    "${TEST_HARNESS_TEMP_DIRECTORY}/Lua.SimpleGaussian.nrrd" )

  sitk_add_lua_test( Example.DicomSeriesReader
    "${SimpleITKExamples_SOURCE_DIR}/DicomSeriesReader.lua"
    --compare-MD5
    "${TEST_HARNESS_TEMP_DIRECTORY}/DicomSeriesReader.nrrd"
    "8e65f711d450b6b466c6d76a667ecb72"
      DATA{${SimpleITK_DATA_ROOT}/Input/DicomSeries/,REGEX:Image[0-9]+.dcm}
      "${TEST_HARNESS_TEMP_DIRECTORY}/DicomSeriesReader.nrrd"
    )
endif ( WRAP_LUA )


#
# Ruby Examples
#
if ( WRAP_RUBY )
  sitk_add_ruby_test( Example.SimpleGaussian
    "${SimpleITKExamples_SOURCE_DIR}/SimpleGaussian.rb"
    --compare
      "${TEST_HARNESS_TEMP_DIRECTORY}/Ruby.SimpleGaussian.nrrd"
      DATA{${SimpleITK_DATA_ROOT}/Baseline/Example_SimpleGaussian_2.0.nrrd}
    DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
    "2.0"
    "${TEST_HARNESS_TEMP_DIRECTORY}/Ruby.SimpleGaussian.nrrd" )

endif ( WRAP_RUBY )


#
# TCL Examples
#
if ( WRAP_TCL )

  sitk_add_tcl_test( Example.SimpleGaussian
    "${SimpleITKExamples_SOURCE_DIR}/SimpleGaussian.tcl"
    --compare
      "${TEST_HARNESS_TEMP_DIRECTORY}/Tcl.SimpleGaussian.nrrd"
      DATA{${SimpleITK_DATA_ROOT}/Baseline/Example_SimpleGaussian_2.0.nrrd}
    DATA{${SimpleITK_DATA_ROOT}/Input/RA-Short.nrrd}
    "2.0"
    "${TEST_HARNESS_TEMP_DIRECTORY}/Tcl.SimpleGaussian.nrrd" )

endif ( WRAP_TCL )