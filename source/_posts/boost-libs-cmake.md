---
title: Add Boost Libraries to CMake
date: 2019-10-22 15:25:28
tags: [cmake, boost, cpp]
categories:
- C++
---

Add needed dependencies to **CMakeLists.txt** file as follows:

```cmake
set(Boost_USE_STATIC_LIBS OFF) 
set(Boost_USE_MULTITHREADED ON)  
set(Boost_USE_STATIC_RUNTIME OFF) 

find_package(Boost 1.69.0 COMPONENTS asio filesystem regex) 

if(Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS}) 
    add_executable(progname file1.cxx file2.cxx) 
    target_link_libraries(progname ${Boost_LIBRARIES})
endif()
```

- **Lines 1 to 3**: Turn on/off boost settings.
- **Line 5**: Add boost libs, e.g system, filesystem, etc.
