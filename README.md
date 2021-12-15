How to install:
- create env 3.6.9
- go to the env:
    ```
    pip install -r requirements.txt
    ./install_all.sh
    pip install pybind11
    ```


note: 
if `git clone` with no `--recursive` then need to run `./install_all.sh` one time to download the necessary repo `orbslam2`, `pangolin` and `g2o`. 
It is highly possible to have some error messages then refer to following.

note: if something wrong with the pybind11 said cannot find python.h

Solution:
- delete the `build`/`bin` folders in each third party folders 
- orbslam2_features:
add `find_package(PythonInterp 3.6 REQUIRED)
find_package(PythonLibs 3.6 REQUIRED)
` to
`thirdparty/orbslam2_features/pybind11/CMakeLists.txt` at line 28-29
- pangolin:
add `find_package(PythonInterp 3.6 REQUIRED)
find_package(PythonLibs 3.6 REQUIRED)
` to
`thirdparty/pangolin/CMakeLists.txt` at line 22-23
- g2o:
add `find_package(PythonInterp 3.6 REQUIRED)
find_package(PythonLibs 3.6 REQUIRED)
` to
`thirdparty/g2opy/EXTERNAL/pybind11/CMakeLists.txt` at line 28-29,
also to `thirdparty/g2opy/EXTERNAL/CMakeLists.txt` top line. 
For some reasons, the file `thirdparty/g2opy/python/CMakeLists.txt` is changed automatically.
Just undo everything back like following
    ```
    include_directories(${PROJECT_SOURCE_DIR})
    
    include_directories(${EIGEN3_INCLUDE_DIR})
    include_directories(${CHOLMOD_INCLUDE_DIR})
    include_directories(${CSPARSE_INCLUDE_DIR})
    
    
    # pybind11 (version 2.2.1)
    LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/EXTERNAL/pybind11/tools)
    include_directories(${PROJECT_SOURCE_DIR}/EXTERNAL/pybind11/include)
    include(pybind11Tools)
    
    
    pybind11_add_module(g2o g2o.cpp)
    target_link_libraries(g2o PRIVATE 
        core
        solver_cholmod
        solver_csparse
        solver_eigen
        solver_dense
        solver_pcg
        solver_slam2d_linear
        solver_structure_only
        types_data
        types_icp
        types_sba
        types_sclam2d
        types_sim3
        types_slam2d
        types_slam2d_addons
        types_slam3d
        types_slam3d_addons
        contrib
    )
    
    ```

run
`main_vo.py` or `main_slam.py`


More old documents refer to `README-old`