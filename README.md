# masterdec

One checkout of the whole decompiler family. Each tool lives in its own
repository and is pulled in here as a submodule, so this repo carries no code
of its own — only the set, and the version of each that is known to go
together.

```sh
git clone --recursive https://github.com/zecoxao/masterdec.git
```

Already cloned without `--recursive`:

```sh
git submodule update --init --recursive
```

Move every submodule to the tip of its own default branch:

```sh
git submodule update --remote --merge
```

## The family

| Submodule | Target | Console |
|---|---|---|
| [`spudec`](https://github.com/zecoxao/spudec) | Cell **SPU** | PS3 |
| [`startrekdec`](https://github.com/zecoxao/startrekdec) | **Kirk** and **Spock** crypto co-processors | PSP |
| [`f00ddec`](https://github.com/zecoxao/f00ddec) | **F00D** security processor (Toshiba MeP) | PS Vita |
| [`rl78dec`](https://github.com/zecoxao/rl78dec) | Renesas **RL78** syscon | PS4 / PS5 |
| [`mechadec`](https://github.com/zecoxao/mechadec) | Sony **SPC970** MechaCon | PS2 |

## What they have in common

All five take the same shape. A decoder produces instructions, a lifter turns
them into a machine-independent IR, and everything above that is shared in
design:

```
decoder -> lifter -> cfg -> opt -> structure -> cgen
```

Each ships the same two front ends: an IDA plugin and a headless CLI that needs
no IDA at all. The IDA hotkeys are the same everywhere —

```
Ctrl-Shift-S   decompile the function under the cursor
Ctrl-F5        decompile everything
```

— and each repo has a test suite that runs without IDA and without a firmware
dump.

Where they differ is the decoder, and that difference is the interesting part:

* **spudec** leans on IDA's own SPU processor module as a pure decoder and
  builds SSA above it; the others carry their own decoders.
* **f00ddec** pairs with [`mep-ida`](https://github.com/zecoxao/mep-ida), a
  Python Toshiba MeP processor module for IDA 9.x.
* **mechadec** is the odd one out: SPC970 is a *prefix machine* with eight
  dispatch tables, where a byte means nothing on its own and the operand width
  belongs to the operation rather than the prefix. It ships its own IDA
  processor module and ROM loader, both driven by the same decoder the
  decompiler uses.

## Not included

No firmware dumps. None of these repositories redistribute one, and every test
suite is written to pass without one.

Two adjacent repos are deliberately not submodules here, because they are
processor modules and loaders rather than decompilers:
[`mep-ida`](https://github.com/zecoxao/mep-ida) and
[`prxldr_python`](https://github.com/zecoxao/prxldr_python).

## Licence

Each submodule carries its own licence (MIT throughout at the time of writing).
