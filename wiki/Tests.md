The test suite requires a data folder `esim_test_data`. Before running any test, it is necessary to export the path to the test data folder as follows (you can add this line to the `setupeventsim.sh` script created previously):

    export ESIM_TEST_DATA_PATH=~/sim_ws/src/event_camera_simulator/event_camera_simulator/esim_test_data/

### Running the tests

    ssim
    roscd
    catkin run_tests esim_common
