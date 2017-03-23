# Using Valgrind

### Valgrind Output
Running Valgrind:
```
valgrind --track-origins=yes --leak-check=full ./shell-app
```
Output is seen in the Valgrind-Output directory, two errors were initially there. First error was in main.cpp, where bool ```terminator``` was initialized but not given a true or false value. The following if statement used this uninitialized value as a condition which lead to the ```Conditional jump or move depends on uninitialised value(s)``` error from valgrind. 

The second error was a memory leak in AnalogSensor.cpp in the Read() function, and was caused by line 16: ```std::vector<int> *readings = new std::vector<int>(mSamples, 10);```
For the dynamic storage,  ```delete readings``` had to be added to prevent the memory leak. This line was added right before ```return result```

After making these changes, running valgrind on the program yielded zero errors. The output after changes are in New-Output.txt in the Valgrind-Output directory. 

### Valgrind as a function and memory profiler
Valgrind can be used with KCachegrind to get a GUI visuilization of runtime and memory problems.

Run the following to generate a callgrind.out file in the same directory
```
valgrind --tool=callgrind ./shell-app
```
In the directory, run ```kcachegrind``` to use the callgrind.out file with the GUI tool.

A screenshot of the KCachegrind GUI is included in this repository. 

### Build
This example was built using:
```
mkdir build
cd build
cmake ..
make
```
Valgrind was run in the ```build/app``` directory.



