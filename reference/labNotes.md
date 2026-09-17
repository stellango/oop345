# OOP345 - General Notes:       Lab Notes

## Compiling and Testing Program

Matrix command:
```
/usr/local/gcc/10.2.0/bin/g++ -Wall -std=c++17 -g -o ws file1.cpp file2.cpp ...
```

Memory leak command:
```
valgrind --show-error-list=yes --leak-check=full --show-leak-kinds=all --track-origins=yes ws
```

## Submission commands

~fardad.soleimanloo/submit 345/w/lab_NAA
~fardad.soleimanloo/submit 345/w/ref_NAA

~fardad.soleimanloo/submit 345/w/asgn -feedback
~fardad.soleimanloo/submit 345/w/ref_NAA -feedback

~fardad.soleimanloo/submit 345/w/lab_NAA -due
~fardad.soleimanloo/submit 345/w/ref_NAA -due

~fardad.soleimanloo/submit 345/prj/m -feedback
~fardad.soleimanloo/submit 345/prj/m


