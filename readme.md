mapf-IR
===
[![MIT License](http://img.shields.io/badge/license-MIT-blue.svg?style=flat)](LICENSE)

A simulator and visualizer of Multi-Agent Path Finding (MAPF), used in a paper ["Iterative Refinement for Real-Time Multi-Robot Path Planning"](https://arxiv.org/abs/2102.12331) (to appear at IROS-21).
It is written in C++(17) with [CMake](https://cmake.org/) (≥v3.16) build.
The repository uses [Google Test](https://github.com/google/googletest) and [the original library for 2D pathfinding](https://github.com/Kei18/grid-pathfinding) as git submodules.
The visualizer uses [openFrameworks](https://openframeworks.cc) and works only on macOS.

The implementations include: HCA\* and WHCA\* [1], PIBT [2], CBS [3], ICBS [4], ECBS [5], Revisit Prioritized Planning [6], Push and Swap [7], winPIBT [8], PIBT+, and IR (Iterative Refinement).

| platform | status (public) | status (dev) |
| ---: | :--- |:--- |
| macos-10.15 | ![test_macos](https://github.com/Kei18/mapf-IR/workflows/test_macos/badge.svg?branch=public) ![build_visualizer_macos](https://github.com/Kei18/mapf-IR/workflows/build_visualizer_macos/badge.svg?branch=public) | ![test_macos](https://github.com/Kei18/mapf-IR/workflows/test_macos/badge.svg?branch=dev) ![build_visualizer_macos](https://github.com/Kei18/mapf-IR/workflows/build_visualizer_macos/badge.svg?branch=dev) |
| ubuntu-latest | ![test_ubuntu](https://github.com/Kei18/mapf-IR/workflows/test_ubuntu/badge.svg?branch=public) | ![test_ubuntu](https://github.com/Kei18/mapf-IR/workflows/test_ubuntu/badge.svg?branch=dev) |

You can see the performance of each solver from [auto\_record repo](https://github.com/Kei18/mapf-IR/tree/auto_record). The records were created by Github Actions.

Please cite the following paper if you use the code in your published research:
```
@inproceedings{okumura2021iterative,
  author={Okumura, Keisuke and Tamura, Yasumasa and Défago, Xavier},
  booktitle={2021 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  title={Iterative Refinement for Real-Time Multi-Robot Path Planning},
  year={2021},
  pages={9690-9697},
  doi={10.1109/IROS51168.2021.9636071}
}
```

## Demo
![100 agents in arena](/material/arena_100agents.gif)

100 agents in arena, planned by PIBT in ~~67ms~~ 5ms.

![1000 agents in brc202d](/material/brc202d_1000agents.gif)

1000 agents in brc202d, planned by PIBT in ~~84sec~~ 1348ms.
The gif shows a part of an MAPF plan.


## Building

```sh
git clone --recursive https://github.com/Kei18/mapf-IR.git
cd mapf-IR
mkdir build
cd build
make

vcs import < mapf_repos.txt
wget -O third_party/openFrameworks.tar.gz https://github.com/openframeworks/openFrameworks/releases/download/0.11.2/of_v0.11.2_linux64gcc6_release.tar.gz
tar -xzvf third_party/openFrameworks.tar.gz -C third_party --strip-components=1 --one-top-level=openFrameworks
sudo third_party/openFrameworks/scripts/linux/ubuntu/install_dependencies.sh
sudo third_party/openFrameworks/scripts/linux/ubuntu/install_codecs.sh
cmake ..
make
```

## Visualizer replay (from mapf-IR root dir)
```sh
./mapf-visualizer/bin/mapf-visualizer ./mapf-visualizer/assets/random-32-32-20.map ./mapf-visualizer/assets/demo_random-32-32-20.txt
./mapf-visualizer/bin/mapf-visualizer ./mapf-visualizer/assets/ost003d.map ./mapf-visualizer/assets/demo_ost003d.txt
./mapf-visualizer/bin/mapf-visualizer ./mapf-visualizer/assets/tunnel.map ./mapf-visualizer/assets/demo_tunnel.txt
```

### for M1 CPU

```sh
cmake -DCPU=M1 ..
```

## Usage
PIBT
```sh
./app -i ../instances/sample.txt -s PIBT -o result.txt -v
```

IR (the result will be saved in result.txt)
```sh
./app -i ../instances/random-32-32-20_70agents_1.txt -s IR_HYBRID -n 300 -t 100 -v
```

You can find details and explanations for all parameters with:
```sh
./app --help
```

Please see `instances/sample.txt` for parameters of instances, e.g., filed, number of agents, time limit, etc.

### Output File

This is an example output of `../instances/sample.txt`.
Note that `(x, y)` denotes location.
`(0, 0)` is the left-top point.
`(x, 0)` is the location at `x`-th column and 1st row.
```
instance= ../instances/sample.txt
agents=100
map_file=arena.map
solver=PIBT
solved=1
soc=3403
makespan=68
comp_time=58
starts=(32,21),(40,4),(20,22),(26,18), [...]
goals=(10,16),(30,21),(11,42),(44,6), [...]
solution=
0:(32,21),(40,4),(20,22),(26,18), [...]
1:(31,21),(40,5),(20,23),(27,18), [...]
[...]
```

## Visualizer

### News

A new visualizer [Kei18@mapf-visualizer](https://github.com/kei18/mapf-visualizer) is available.
I recommend using the new one instead of this repo.

### Building
It takes around 10 minutes.

#### macOS
```sh
bash ./visualizer/scripts/build_macos.sh
```

Note: The script of openFrameworks seems to contain bugs. Check this [issue](https://github.com/openframeworks/openFrameworks/issues/6623). I fixed this in my script :D


### Usage
```sh
cd build
../visualize.sh result.txt
```

You can manipulate it via your keyboard. See printed info.

## Performance History
Generated by Github Actions. See also [auto\_record repo](https://github.com/Kei18/mapf-IR/tree/auto_record).

![sub-optimal solvers](https://github.com/Kei18/mapf-IR/blob/auto_record/fig/transition_0.jpg)

![optimal solvers](https://github.com/Kei18/mapf-IR/blob/auto_record/fig/transition_1.jpg)

## Experimental Environment
[![v1.1](https://img.shields.io/badge/tag-v1.1-blue.svg?style=flat)](https://github.com/Kei18/mapf-IR/releases/tag/v1.1)

Scripts for the experiments are in `exp_scripts/`.
Several solvers are coded in different names. See the following.

| paper | code |
| :--- | :--- |
| RPP | RevisitPP |
| PIBT+ | PIBT_COMPLETE |
| IR: random | IR |
| IR: single-agent | IR\_SINGLE\_AGENTS |
| IR: focusing-at-goals | IR\_FOCUS\_GOALS |
| IR: local-repair-around-goals | IR\_FIX\_AT\_GOALS |
| IR: using-MDD | IR\_MDD |
| IR: using-bottleneck-agent | IR\_BOTTLENECK |
| IR: composition | IR\_HYBRID |

## Notes
- Maps in `maps/` are from [MAPF benchmarks](https://movingai.com/benchmarks/mapf.html).
  When you add a new map, please place it in the `maps/` directory.
- The font in `visualizer/bin/data` is from [Google Fonts](https://fonts.google.com/).

## Licence
This software is released under the MIT License, see [LICENSE.txt](LICENCE.txt).

## Author
[Keisuke Okumura](https://kei18.github.io) is a Ph.D. student at the Tokyo Institute of Technology, interested in controlling multiple moving agents.

## Reference
1. Silver, D. (2005).
    Cooperative pathfinding.
    Proc. AAAI Conf. on Artificial Intelligence and Interactive Digital Entertainment (AIIDE-05)
1. Okumura, K., Machida, M., Défago, X., & Tamura, Y. (2019).
   Priority Inheritance with Backtracking for Iterative Multi-agent Path Finding.
   Proc. Intel. Joint Conf. on Artificial Intelligence (IJCAI)
1. Sharon, G., Stern, R., Felner, A., & Sturtevant, N. R. (2015).
   Conflict-based search for optimal multi-agent pathfinding.
   Artificial Intelligence
1. Boyarski, E., Felner, A., Stern, R., Sharon, G., Tolpin, D., Betzalel, O., & Shimony, E. (2015).
   ICBS: improved conflict-based search algorithm for multi-agent pathfinding.
   Proc. Intel. Joint Conf. on Artificial Intelligence (IJCAI)
1. Barer, M., Sharon, G., Stern, R., & Felner, A. (2014).
   Suboptimal Variants of the Conflict-Based Search Algorithm for the Multi-Agent Pathfinding Problem.
   Annual Symposium on Combinatorial Search (SoCS)
1. Čáp, M., Novák, P., Kleiner, A., & Selecký, M. (2015).
   Prioritized planning algorithms for trajectory coordination of multiple mobile robots.
   IEEE Trans. on automation science and engineering
1. Luna, R., & Bekris, K. E. (2011).
   Push and swap: Fast cooperative path-finding with completeness guarantees.
   Proc. Intel. Joint Conf. on Artificial Intelligence (IJCAI)
1. Okumura, K., Tamura, Y. & Défago, X. (2020).
   winPIBT: Extended Prioritized Algorithm for Iterative Multi-agent Path Finding.
   IJCAI Workshop on Multi-Agent Path Finidng (WoMAPF)
