### General Questions (windows/mac/linux):

#### A1. Where is the win rate?

Print in the log, something like:

<pre>
I0514 12:51:32.724236 14467 mcts_engine.cc:157] 1th move(b): dp, <b>winrate=44.110905%</b>, N=654, Q=-0.117782, p=0.079232, v=-0.116534, cost 39042.679688ms, sims=7132, height=11, avg_height=5.782244, global_step=639200
</pre>

#### A2. Where is the PV (Analysis) ?

It is possible to display the PV (variation of move path with 
continuation of the moves)

An easy way to do that is for example to increase verbose level, 
for example `--logtostderr --v=1` 
(on windows the syntax is different, see 
[FAQ question](/docs/FAQ.md#a4-syntax-error-windows) for details

result is something like this (in this example there are 
7000 simulations per move) :

```
[5489] stderr: I0116 15:55:09.910559  5489 mcts_debugger.cc:43] ========== debug info for 27th move(b) begin ==========
[5489] stderr: I0116 15:55:09.910596  5489 mcts_debugger.cc:44] main move path: mg(6482,-0.12,0.89,-0.09),lf(6349,0.12,0.88,0.08),lg(3006,-0.12,0.53,-0.12),kf(2654,0.14,0.63,-0.01),qh(941,-0.11,0.16,-0.11),qi(785,0.12,0.77,0.13),rh(784,-0.12,0.98,-0.13),pi(538,0.13,0.52,0.09),rg(445,-0.12,0.73,-0.18),og(365,0.13,0.76,0.15),pf(363,-0.13,0.98,-0.19),rd(159,0.10,0.52,0.14),ql(47,-0.06,0.21,-0.18),oq(19,0.04,0.36,-0.03),nq(12,-0.05,0.58,-0.07),pq(11,0.05,0.97,0.03),np(10,-0.05,0.79,-0.12),qn(9,0.04,0.84,0.03),ol(8,-0.05,0.82,-0.05),fc(6,0.07,0.60,0.06),kg(4,-0.06,0.42,-0.13),jf(2,-0.02,0.53,0.02),jg(1,0.05,0.45,0.05),cg(0,-nan,0.00,nan)
[5489] stderr: I0116 15:55:09.910604  5489 mcts_debugger.cc:139] mg: N=6482, W=-757.077, Q=-0.116797, p=0.887549, v=-0.0932888
[5489] stderr: I0116 15:55:09.910609  5489 mcts_debugger.cc:139] oh: N=262, W=-38.6935, Q=-0.147685, p=0.0710239, v=-0.137658
[5489] stderr: I0116 15:55:09.910614  5489 mcts_debugger.cc:139] qh: N=38, W=-6.34157, Q=-0.166883, p=0.0139782, v=-0.144544
[5489] stderr: I0116 15:55:09.910617  5489 mcts_debugger.cc:139] nb: N=1, W=-0.294998, Q=-0.294998, p=0.000126838, v=-0.295011
[5489] stderr: I0116 15:55:09.910621  5489 mcts_debugger.cc:139] lc: N=1, W=-0.354553, Q=-0.354553, p=0.000164667, v=-0.354567
[5489] stderr: I0116 15:55:09.910626  5489 mcts_debugger.cc:139] ob: N=1, W=-0.28775, Q=-0.28775, p=0.000373717, v=-0.287757
[5489] stderr: I0116 15:55:09.910630  5489 mcts_debugger.cc:139] oj: N=1, W=-0.359818, Q=-0.359818, p=0.000128213, v=-0.359819
[5489] stderr: I0116 15:55:09.910634  5489 mcts_debugger.cc:139] rc: N=1, W=-0.30574, Q=-0.30574, p=0.000222438, v=-0.305748
[5489] stderr: I0116 15:55:09.910639  5489 mcts_debugger.cc:139] oc: N=1, W=-0.281403, Q=-0.281403, p=0.000368299, v=-0.281408
[5489] stderr: I0116 15:55:09.910642  5489 mcts_debugger.cc:139] qd: N=1, W=-0.288712, Q=-0.288712, p=0.00012808, v=-0.288724
[5489] stderr: I0116 15:55:09.910646  5489 mcts_debugger.cc:48] model global step: 639200
[5489] stderr: I0116 15:55:09.910648  5489 mcts_debugger.cc:49] ========== debug info for 27th move(b) end   ==========
```

It is also possible to develop all the tree by adding these lines to 
your config file :

```
debugger {
    print_tree_depth: 20
    print_tree_width: 3
}
```

In this example, a tree of 20 depth 3 width moves will be printed

#### A2.5 How to analyze/review one or many sgf file(s) with GoReviewPartner

See [this document](/docs/go-review-partner.md)

#### A3. There are too much log.

Passing `--v=0` to `mcts_main` will turn off many debug log.
Moreover, `--minloglevel=1` and `--minloglevel=2` could disable 
INFO log and WARNING log.

Or, if you just don't want to log to stderr, replace `--logtostderr` 
to `--log_dir={log_dir}`, then you could read your log from 
`{log_dir}/mcts_main.INFO`.

#### A4. Syntax error (Windows)

For windows,
- in config file, 

you need to write path with `/` and not `\` in the config 
file .conf, for example : 

```
model_config {
      train_dir: "c:/users/amd2018/Downloads/PhoenixGo/ckpt"
```

- in cmd.exe,

Here you need to write paths with `\` and not `/`. 
Also command format on windows needs a space and not a `=`, for 
example : 

`mcts_main.exe --gtp --config_path C:\Users\amd2018\Downloads\PhoenixGo\etc\mcts_1gpu_notensorrt.conf`

or if you want to show the PV, you need to remove the `=` too, 
for example : 

`mcts_main.exe --gtp --config_path C:\Users\amd2018\Downloads\PhoenixGo\etc\mcts_1gpu_notensorrt.conf --logtostderr --v 1`

See next point below :

#### A5. '"ckpt/zero.ckpt-20b-v1.FP32.PLAN"' error: No such file or directory

This fix works for all systems : Linux, Mac, Windows, only the 
name of the ckpt file changes. Modify your config file and write 
the full path of your ckpt directory, for example for linux : 

```
model_config {
    train_dir: "/home/amd2018/PhoenixGo/ckpt"
```
    
for example, for windows :

```
model_config {
    train_dir: "c:/users/amd2018/Downloads/PhoenixGo/ckpt"
```

if you use tensorRT (linux only, and compatible nvidia GPU 
only), also change path of tensorRT, for example :

```
model_config {
    train_dir: "/home/amd2018/PhoenixGo/ckpt/"
    enable_tensorrt: 1
    tensorrt_model_path: "/home/amd2018/test/PhoenixGo/ckpt/zero.ckpt-20b-v1.FP32.PLAN"
}
```

#### A6. How to run with Sabaki?

Setting GTP engine in Sabaki's menu: `Engines -> Manage Engines`, 
fill `Path` with path of `start.sh`.
Click `Engines -> Attach` to use the engine in your game.
See also [#22](https://github.com/Tencent/PhoenixGo/issues/22).

#### A7. How make PhoenixGo think with longer/shorter time?

Modify `timeout_ms_per_step` in your config file.

#### A8. How make PhoenixGo think with constant time per move?

Modify your config file. `early_stop`, `unstable_overtime`, 
`behind_overtime` and`time_control` are options that affect the 
search time, remove them if exist then
each move will cost constant time/simulations.

#### A9. What is the speed of the engine ? How can i make the engine think faster ?

GPU is much faster to compute than CPU (but only nvidia GPU are 
supported)

TensorRT also increases significantly the speed of computation, 
but it is only available for linux with a compatible nvidia GPU

Bigger batch size significantly increases the speed of the computation, 
but a bigger batch size puts a bigger burden on the computation device 
(in case it is the GPU, higher GPU load, higher VRAM usage), increase 
it only if your computation device can handle it

Some independent speed benchmarks have been run, they are available 
in the docs :

- for GTX 1060 : 
[benchmark testing batch size from 4 to 64, tree size up to 2000M, max children up to 512, with tensorRT ON and OFF](/docs/benchmark-gtx1060.md)

#### A10. GTP command `time_settings` doesn't work.

Add these lines in your config:

```
time_control {
    enable: 1
    c_denom: 20
    c_maxply: 40
    reserved_time: 1.0
}
```

- `c_denom` and `c_maxply` are parameters for deciding how to use the 
"main time".
- `reserved_time` is how many seconds should reserved (for network latency) 
in "byo-yomi time".

#### A11. GTP command error : `invalid command`

Some GTP commands are not supported by PhoenixGo, for example the 
`showboard` command. 

To know supported GTP commands, start phoenixgo in GTP mode and enter 
the GTP command `list_commands`

Result as of today is : 

```
version
protocol_version
list_commands
quit
clear_board
boardsize
komi
time_settings
time_left
place_free_handicap
set_free_handicap
play
genmove
final_score
get_debug_info
get_last_move_debug_info
```

If you use unsupported commands the engine will not work. Make sure your 
GTP tool does not communicate with PhoenixGo with unsupported GTP commands.

For example, for [gtp2ogs](https://github.com/online-go/gtp2ogs) server 
command line GTP tool, you need to edit the file gtp2ogs.js and manually 
remove the existing `showboard` line if it 
is not already done, 
[see](https://github.com/online-go/gtp2ogs/commit/d5ebdbbde259a97c5ae1aed0ec42a07c9fbb2dbf)

#### A12. The game does not start, error : `unacceptable komi`

With the default settings, the only komi value supported is only 7.5, with 
chinese rules only.If it is not automated, you need to manually set komi 
value to 7.5 with chinese rules.

If you want to implement PhoenixGo to a server where players play with 
different komi values (for example 6.5, 0.5, 85.5, 200.5,etc), you need 
to force komi to 7.5 for the players. 

If it is not possible, there is a workaround you can use : configure 
you GTP tool to tell PhoenixGo engine that the komi for the game is 
7.5 even if it is not true

if you do that, the game will not be scored correctly because PhoenixGo 
will think that the komi is 7.5 while the real komi is different, but at 
least PhoenixGo will be able to play the game.
 
An example for [gtp2ogs](https://github.com/online-go/gtp2ogs) is provided 
[here](https://github.com/wonderingabout/gtp2ogs-tutorial/blob/master/docs/3A4-linux-optional-edit-gtp2ogs-js-file.md) 
and [here](https://github.com/wonderingabout/gtp2ogs-tutorial/blob/master/docs/3B4-windows-optional-edit-gtp2ogs-js-file.md)

#### A13. I have a nvidia RTX card (Turing) or Tesla V100/Titan V (Volta), is it compatible ?

##### RTX cards (Turing) :

- need CUDA 10.0 or higher (so currently, only linux is supported, or 
windows with your own building)
- If you compile PhoenixGo, it has been tested to work on linux 
[here](/docs/tested-versions.md) with CUDA 10.0, cudnn 7.4.2, ubuntu 18.04.
- However currently there is no tensorRT support for PhoenixGo 
(RTX cards require tensorRT 5.x or more, and this also requires tensorflow 1.9 or more), 
which is currently not supported by PhoenixGo)

##### Volta cards (Tesla V100 / Titan V and similar)

- are compatible with cuda 9.0 and higher (it is recommended to 
use latest version when possible),
cudnn 7.1.x or higher (x is any number)
- has been tested to work successfully on windows
- to use TensorRT with V100, you need to manually build TensorRT 
model on V100.
See: [#75](https://github.com/Tencent/PhoenixGo/issues/75) for 
how to build TensorRT model.

### Specific questions : bazel issues (linux and mac)

#### B0. It is too hard to install bazel or start bazel

You may find hard to download, install, run bazel

If that's the case, and if you are using ubuntu or similar 
operating system, 
you can use the all-in-one command below instead of the 
[main README](/README.md) commands

Read the [main README](/README.md) for explanations and instructions : 
the all-in-one command below only saves you the time to find how to 
install bazel and run all the bazel commands one by one, but you 
still have to read the [main README](/README.md) for explanations

The all-in-one command below has been tested to run successfully on 
ubuntu 16.04 LTS and 18.04 LTS

```
sudo apt-get -y install pkg-config zip g++ zlib1g-dev unzip python git && \
git clone https://github.com/Tencent/PhoenixGo.git && \
cd PhoenixGo && \
wget https://github.com/bazelbuild/bazel/releases/download/0.11.1/bazel-0.11.1-installer-linux-x86_64.sh && \
chmod +x bazel-0.11.1-installer-linux-x86_64.sh && \
./bazel-0.11.1-installer-linux-x86_64.sh --user && \
echo 'export PATH="$PATH:$HOME/bin"' >> ~/.bashrc && source ~/.bashrc && \
sudo ldconfig && \
wget https://github.com/Tencent/PhoenixGo/releases/download/trained-network-20b-v1/trained-network-20b-v1.tar.gz && \
tar xvzf trained-network-20b-v1.tar.gz && \
rm trained-network-20b-v1.tar.gz bazel-0.11.1-installer-linux-x86_64.sh && \
./configure && \
bazel build //mcts:mcts_main
```

This all-in-one command will : 

- Download and install bazel and PhoenixGo dependencies for ubuntu 
and similar systems (need `apt-get`)
- Clone PhoenixGo from github
- Download and install bazel 0.11.1
- Do the post-install of bazel
- Download and extract trained network (ckpt)
- Cleanup : trained network archive and remove bazel installer
- Run `./configure` : at this step, you have to confifure bazel 
same as explained in [main README](/README.md)
- When configure is finished, start building automatically, 
same as explained in [main README](/README.md)

#### B1. I am getting errors during bazel configure, bazel building, and/or running PhoenixGo engine

If you built with bazel, see : 
[Most common path errors during cuda/cudnn install and bazel configure](/docs/path-errors.md)

See also [minimalist bazel configure](/docs/minimalist-bazel-configure.md) 
for an example of build configure

If you are still getting errors, try using an older version of bazel. 
For example bazel 0.20.0 is known to cause issues, and 
**bazel 0.11.1 is known good**

#### B2. The PhoenixGo and bazel folders are too big

During the bazel building, there are many options that can are not 
required and can be disabled

This will reduce building time and will have smaller size after the building, 
see [minimalist bazel configure](/docs/minimalist-bazel-configure.md) and 
[#76](https://github.com/Tencent/PhoenixGo/issues/76)

#### B3. I cannot increase batch size to more than 4 with TensorRT (linux only)

Increasing batch size in the config file makes the engine compute faster, 
as explained earlier in 
[FAQ question](/docs/FAQ.md/#6-what-is-the-speed-of-the-engine--how-can-i-make-the-engine-think-faster-)

However, with default building, you cannot use batch size higher than 4 
with tensorRT
To increase batch size for example to 32 with tensorRT enabled, you need 
to build tensorrt model with bazel, See : 
[#75](https://github.com/Tencent/PhoenixGo/issues/75)

#### B4. How to remove entirely all PhoenixGo and bazel files and folders ?

assuming you installed PhoenixGo in home directory 
(`~` or `/home/yourusername/`)

```
# clean with bazel
cd ~/PhoenixGo && bazel clean
# remove PhoenixGo local directory
sudo rm -rf ~/PhoenixGo
# remove any remaining bazel file
sudo rm -rf ~/.cache/bazel
```
This will free a few GB (arround 3-6 GB depending on your installation 
settings)
