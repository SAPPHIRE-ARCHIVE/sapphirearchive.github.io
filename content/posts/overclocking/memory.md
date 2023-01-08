---
title: Memory Overclocking
---

## Guides:

#### [MemTestHelper DDR4 OC Guide](https://github.com/integralfx/MemTestHelper/blob/oc-guide/DDR4%20OC%20Guide.md)

## Stress Tests

#### [TestMem5](https://www.overclock.net/threads/memory-testing-with-testmem5-tm5-with-custom-configs.1751608/)
This is the best option for stressing DDR4 memory.
- Run and load the anta777 Extreme1 config, and make sure the application is running as admin for stressing memory.
- 3 cycles is good enough to test individual timings while tightening, but prefferably try to get to 9 at least for the final stress test.

#### [y-crucher](http://www.numberworld.org/y-cruncher/#Download)
- Run application and select "Component Stress Tester" (option 1).
- Select "Start stress testing" (option 0).
- Run for 4 iterations. This is good at picking out errors that the main stress test may not., especially on Alderlake CPUs.

#### [Karhu](https://www.karhusoftware.com/ramtest/)
This is the best option for stressing DDR5 memory, but keep in mind that you have to purchase a license to use it.
- Enable CPU Cache in settings.
- Try to aim for around 6400% coverage minimum for each timing change, with at least 10000% for the final OC.
