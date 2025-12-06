# cloacked-pixel-oc-python3

An overcharged version of [Grazee/cloacked-pixel-python3](https://github.com/Grazee/cloacked-pixel-python3) with much higher performance (see [#performance](#performance)).

> As **NOBODY** is using python2, so. A python3 version of [cloacked-pixel](https://github.com/livz/cloacked-pixel.git).

## Installation

Follow these shell commands:

```shell
# clone this repo to local
git clone https://github.com/Grazee/cloacked-pixel-python3.git

# install python3 dependencies
pip3 install -r requirements.txt
```

## Usage

### Hide

To hide data or file into an image:

```shell
python3 lsb.py hide -i [img_file] -s [payload_file] -o [out_file] -p [password]
```

**For example**, hide `secret.png` into `origin.jpg` with password `1234567`.

```shell
python3 lsb.py hide -i example/origin.jpg -s example/secret.png -o stego.png -p 1234567
```

The command above will generate a new file named `stego.png`.

### Extract

To extract data or file from an stego image:

```shell
python3 lsb.py extract -i [stego_file] -o [out_file] -p [password]
```

**For example**, extract secret data from `stego.jpg` with password `1234567`, and save those data as file `secret.png`:

```shell
python3 lsb.py extract -i example/stego.png -o secret.png -p 1234567
```

## Performance

Timing is measured using [hyperfine](https://github.com/sharkdp/hyperfine): `hyperfine --warmup 10 --runs 25 "$command"`. Memory is measured using `valgrind --tool=massif --massif-out-file=/tmp/massif.out "$command"; grep mem_heap_B /tmp/massif.out | sed -e 's/mem_heap_B=\(.*\)/\1/' | sort -g | tail -n 1`.

All numbers are measured on an AMD Ryzen 9 9950X workstation kindly shared by [@woshiluo](https://github.com/woshiluo), running Python 3.13.7.

### Extraction

Command: `python3 ./lsb.py extract -i "$image" -o /dev/null -p 1234567`

#### 640x640 -> 140x84

| Version                                                                                                                                        | Time (ms)       | Heap (MiB)    |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ------------- |
| [`Grazee/cloacked-pixel-python3@f309738bac`](https://github.com/Grazee/cloacked-pixel-python3/commit/f309738bacc3146b97bfc03f5887aa319de39e67) | 418.5 ± 4.0     | 27.841865     |
| This repo                                                                                                                                      | **320.6 ± 4.2** | **27.744255** |

#### 2500x2500 -> 256x256

| Version                                                                                                                                        | Time (ms)     | Heap (MiB)     |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | -------------- |
| [`Grazee/cloacked-pixel-python3@f309738bac`](https://github.com/Grazee/cloacked-pixel-python3/commit/f309738bacc3146b97bfc03f5887aa319de39e67) | 61195 [^1]    | 219.444520     |
| This repo                                                                                                                                      | **2035 ± 40** | **217.583819** |

### Hiding

Command: `python3 ./lsb.py hide -i "$image_1" -s "$image_2" -o /dev/null -p 1234567`

#### 640x640 + 140x84 -> 640x640

| Version                                                                                                                                        | Time (ms)       | Heap (MiB)    |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ------------- |
| [`Grazee/cloacked-pixel-python3@f309738bac`](https://github.com/Grazee/cloacked-pixel-python3/commit/f309738bacc3146b97bfc03f5887aa319de39e67) | **324.9 ± 2.9** | **21.480788** |
| This repo                                                                                                                                      | 326.8 ± 3.1     | 21.484233     |

[^1]: This is measured using `time`, otherwise it would take too long.
